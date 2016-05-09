task :default => :new

require 'fileutils'

task :new do
puts "title: "
@title = STDIN.gets.chomp

@date = Time.now.strftime("%F")
@post_name = "_posts/#{@date}-#{@title}.md"

if File.exist?(@post_name)
abort("文件名已经存在，创建失败！")
end 

FileUtils.touch(@post_name)
open(@post_name, 'a') do |file|
file.puts "---"
file.puts "layout:     post"
file.puts "title:      \"#{@title}\""
file.puts "subtitle:   \"\""
file.puts "date:       #{Time.now}"
file.puts "author:     \"Liyf\""
file.puts "header-img: \"img/\""
file.puts "catalog:    true"
file.puts "tags: "
file.puts "    - "
file.puts "---"
end

exec "vi #{@post_name}"
end