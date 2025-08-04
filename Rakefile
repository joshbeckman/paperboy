# frozen_string_literal: true

require 'fileutils'
require 'json'
require 'readwise'
require 'time'
require 'uri'

# Helper methods for Readwise document export
module ReadwiseExporter
  def self.prepare_directory(dir)
    FileUtils.rm_rf(dir) if Dir.exist?(dir)
    FileUtils.mkdir_p(dir)
  end

  def self.generate_filename(doc)
    title_slug = doc.title.to_s
                    .downcase
                    .gsub(/[^a-z0-9]+/, '-')
                    .gsub(/^-+|-+$/, '')
                    .slice(0, 100) # Limit length

    updated_timestamp = Time.parse(doc.updated_at).strftime('%Y%m%d%H%M%S')
    "#{updated_timestamp}-#{title_slug}.json"
  end

  def self.export_documents(docs, output_dir)
    exported_count = 0

    docs.each do |doc|
      filename = generate_filename(doc)
      filepath = File.join(output_dir, filename)
      doc_hash = doc.serialize
      doc_hash[:published_at] = Time.at(doc.published_date / 1000).iso8601 if doc.published_date&.is_a?(Integer)
      begin
        doc_hash[:domain] = URI.parse(doc.source_url).host if doc.source_url
      rescue URI::InvalidURIError, URI::InvalidComponentError
        doc_hash[:domain] = 'email'
      end
      fields_to_remove = %i[id created_at updated_at notes html published_date source_url source reading_time reading_progress word_count category parent_id]
      fields_to_remove.each { |field| doc_hash.delete(field) }
      File.write(filepath, JSON.pretty_generate(doc_hash))
      exported_count += 1

      puts "Exported: #{filename}"
    end

    puts "Exported #{exported_count} documents to #{output_dir}/"
    exported_count
  end
end

namespace :readwise do
  desc 'Export Readwise documents updated in the last 2 weeks to recent_reading directory'
  task :export_recent do
    client = Readwise::Client.new(token: ENV.fetch('READWISE_TOKEN', nil))
    two_weeks_ago = (Date.today - 14).to_time
    output_dir = 'recent_reading'

    ReadwiseExporter.prepare_directory(output_dir)

    puts "Fetching documents updated after #{two_weeks_ago.iso8601}..."
    docs = client.get_documents(location: 'archive', updated_after: two_weeks_ago.iso8601)

    ReadwiseExporter.export_documents(docs, output_dir)
  end

  desc 'Export Readwise documents from new and later locations to respective directories'
  task :export_inbox do
    client = Readwise::Client.new(token: ENV.fetch('READWISE_TOKEN', nil))

    %w[new later].each do |location|
      output_dir = location

      ReadwiseExporter.prepare_directory(output_dir)

      puts "\nFetching documents from '#{location}' location..."
      docs = client.get_documents(location: location)

      ReadwiseExporter.export_documents(docs, output_dir)
    end
  end
end

desc 'Export all documents and generate personalized newspaper'
task default: ['readwise:export_inbox', 'readwise:export_recent'] do
  puts "\n📰 Launching Claude to generate your personalized newspaper..."
  system 'claude -p "/paperboy" --model=sonnet'
end
