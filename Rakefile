require "mp3info"
require 'pathname'

desc "Create a new post based off your mp3"
task :new_page do
  name = ARGV.last
  if name == "new_page"
    puts "Please give a file path to an MP3"
    exit
  end

  mp3_path = ARGV.last
  puts "Checking out: " + mp3_path

  length = ""
  Mp3Info.open(mp3_path) do |mp3|
    length = Time.at(mp3.length).utc.strftime("%M:%S")
  end

  filesize = File.stat(mp3_path).size
  filename = Pathname.new(mp3_path).basename

  output = ""
  output <<  "---\n"
  output <<  "layout: post"
  output <<  "podcast_file_name: '#{filename}'\n"
  output <<  "duration: '#{length}'\n"
  output <<  "data_length: '#{filesize}'\n"
  output <<  "published: true\n"
  output <<  "show_notes:\n"
  output <<  '  - "http://thing.com": "OK"\n'
  output <<  "---\n"
  output <<  "\n"
  output <<  "This is a new post"
  
  post_count = Dir.glob(File.join("_posts/", '*')).select { |file| File.file?(file) }.count + 1
  post_filename =  Time.now.strftime("%Y-%d-%m-") + ("%03d" % post_count)
  full_post_path = '_posts/' + post_filename + ".html"
  

  File.open(full_post_path, 'w') do |file|
    file.write(output)
  end
  
  puts "Created file at "+ full_post_path

end
