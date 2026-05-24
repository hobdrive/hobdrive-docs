load '../Rakefile'

task :upload_manuals do
    upload_yc("hd-downloads", "root/user-manual-en.pdf", "user-manual-en.pdf")
    upload_yc("hd-downloads", "root/user-manual-ru.pdf", "user-manual-ru.pdf")
end

task :prep do
    sh "cp ../hobd/changelog_ru ./ru/changelog_ru.md"
    sh "cp ../hobd/changelog_en ./en/changelog_en.md"
end

task :build do
    sh "bundle exec jekyll b"
    Rake::Task[:relativize_urls].invoke
end

task :run_dev do
    sh "bundle exec jekyll s"
end

task :relativize_urls do
  puts "Converting absolute URLs to relative..."
  
  Dir.glob("_site/**/*.html").each do |file|
    content = File.read(file)
    
    # Calculate depth based on file path relative to _site
    relative_path = file.sub('_site/', '')
    depth = relative_path.split('/').size - 1
    depth = 0 if depth < 0
    prefix = depth > 0 ? ('../' * depth) : './'
    
    # Convert absolute internal paths to relative
    modified = content.gsub(/(href|src)="\/([^"]*)"/) do
      attr = $1
      path = $2
      
      # Skip if it's a protocol-relative URL (starts with //)
      next "#{attr}=\"/#{path}\"" if path.start_with?('/')
      
      # For root path
      if path.empty?
        "#{attr}=\"#{prefix.chomp('/')}\""
      else
        "#{attr}=\"#{prefix}#{path}\""
      end
    end
    
    File.write(file, modified) if modified != content
  end
  
  puts "Done!"
end