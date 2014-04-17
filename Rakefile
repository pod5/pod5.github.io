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
  post_filename =  Time.now.strftime("%Y-%m-%d") + ("-episode%d" % post_count)
  full_post_path = '_posts/' + post_filename + ".html"

  output = ""
  output <<  "---\n"
  output <<  "layout: post\n"
  output <<  "podcast_file_name: '#{filename}'\n"
  output <<  "duration: '#{length}'\n"
  output <<  "data_length: '#{filesize}'\n"
  output <<  "published: true\n"
  output <<  "title: Episode #{post_count}\n"
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

  @pods = get_pods_from_args
end



desc "Create a summary of pod authors for a tweet."
task :tweet do
  summary = "We'd like to thank "
  pods_with_no_attribution = []

  @pods.each do |pod|
    if pod["social_media_url"]
      summary << "@" + pod["social_media_url"].split("/")[-1] + ", "
    else
      pods_with_no_attribution << pod
    end
  end

  puts summary + "\n\n has been added to your pasteboard."
  IO.popen('pbcopy', 'w') { |f| f << summary }

  if pods_with_no_attribution.length
    puts "\n\nCould not find twitter deets for:"
    pods_with_no_attribution.each do |pod|
      puts " - " + pod["homepage"]
    end
  end

  exit
end



desc "Create a summary of pod authors for the website."
task :html do
  summary = "{% include post_start.html %}\n\n"
  @pods.each do |pod|
    authors = authors(pod)

    summary += "{% include note.html " +
      "  project='" + pod["name"] +
      "' version='" + pod["version"] +
      "' author='"  + authors +
      "' twitter='" + (pod["social_media_url"] ? pod["social_media_url"].split("/")[-1] : "") +
      "' description='"   + pod["summary"].gsub("'", "&#39;") +
      "' homepage='" + pod["homepage"] +
      "' %} \n\n"
  end
  summary += "{% include post_end.html %}\n\n"

  puts summary + "\n\n has been added to your pasteboard."
  IO.popen('pbcopy', 'w') { |f| f << summary }

  exit
end


def get_pods_from_args

  require 'open-uri'
  require 'json'

  # This will someday break
  #  https://github.com/CocoaPods/search.cocoapods.org/blob/8908eaaf83a11b3075d36032cb8c5896e37c7366/app.rb#L119-L121
  puts "Downloading podspecs \n\n"

  pods = []
  ARGV.reverse_each do |pod_name|
    break if ["tweet", "pod_name", "html"].include? pod_name
    begin
      content = open("http://search.cocoapods.org/api/v1/pod/" + pod_name + ".json").read
      pods << JSON.parse(content)
    rescue Exception => e
      p pod_name + " " + e.to_s
    end
  end
  pods.reverse
end

def authors pod
  authors = pod["authors"] || pod["author"]
  if authors.is_a?(Hash)
    authors.keys.join(" ")
  elsif authors.is_a? Array
    authors.join(" ")
  else
    authors
  end
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
  exec "s3cmd --config=.s3cfg --acl-public put '#{ name }'  s3://#{config["podcast_file_bucket"]}/episodes/#{filename}"
end
