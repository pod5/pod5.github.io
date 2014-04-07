require "mp3info"
require 'pathname'

desc "Create a new post based off your mp3"
task :new_podcast do
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

  post_count = Dir.glob(File.join("_posts/", '*')).select { |file| File.file?(file) }.count + 1
  post_filename =  Time.now.strftime("%Y-%m-%d") + ("%03d" % post_count)
  full_post_path = '_posts/' + post_filename + ".html"

  output = ""
  output <<  "---\n"
  output <<  "layout: post\n"
  output <<  "podcast_file_name: '#{filename}'\n"
  output <<  "duration: '#{length}'\n"
  output <<  "data_length: '#{filesize}'\n"
  output <<  "published: true\n"
  output <<  "title: Episode #{post_count}\n"
  output <<  "show_notes:\n"
  output <<  '  - "http://thing.com": "OK"\n'
  output <<  "---\n"
  output <<  "\n"
  output <<  "This is a new post"
  

  File.open(full_post_path, 'w') do |file|
    file.write(output)
  end
  
  puts "Created file at "+ full_post_path

end

desc "Create a summary of each pod"
task :pod_stats do
  name = ARGV.last
  if name == "pod_stats"
    puts "Please give some pods."
    exit
  end

  # This will someday break
  #  https://github.com/CocoaPods/search.cocoapods.org/blob/8908eaaf83a11b3075d36032cb8c5896e37c7366/app.rb#L119-L121
  
  require 'open-uri'
  require 'json'
  
  pods = []
  ARGV.reverse_each do |pod_name| 
    break if pod_name == "pod_stats"
      
    puts "Getting #{pod_name}" 
    content = open("http://search.cocoapods.org/api/v1/pod/" + pod_name + ".json").read
    pods << JSON.parse(content)
  end
  
  summary = ""  
  pods.reverse_each do |pod|
    summary << "<p><a href=" + pod["homepage"] + '>' + pod["name"] + "</a> " + pod["version"] + " by " + pod["authors"].keys.first + "<br/>\n"
    summary << " &mdash; " + pod["summary"] + "</p>\n"
  end
  
  puts summary
  
  exit
end

desc "Upload an MP3 to your s3 account"
task :upload_mp3 do
  name = ARGV.last
  if name == "upload_mp3"
    puts "Please give a file path to an MP3"
    exit
  end
  
  require 'yaml'
  config = YAML.load File.open("_config.yml").read
  filename = Pathname.new(name).basename
  exec "s3cmd --config=.s3cfg --acl-public put #{ name }  s3://#{config["podcast_file_bucket"]}/episodes/#{filename}" 
end