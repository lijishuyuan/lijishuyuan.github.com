---
layout: post
title: "Jekyll Bootstrap博客框架搭建总结"
---

jekyll的中文标题转换拼音解决方案   
http://blog.turbidsoul.me/2013/05/23/jekyll-de-zhong-wen-biao-ti-zhuan-huan-pin-yin-jie-jue-fang-an/  
这个方法在windows试了几次没成功，想想也没必要，rake 英文即可，生成的文件改变post里边的title即可。


经过不懈努力终于可以在win7下rake 中文了，除了上边说的在rakefile修改  


	# encoding: utf-8
	require "rubygems"
	require 'rake'
	require 'yaml'
	require 'time'
	require 'hz2py'

	task :post do
	  abort("rake aborted: '#{CONFIG['posts']}' directory not found.") unless FileTest.directory?(CONFIG['posts'])
	  title = ENV["title"] || "new-post"
	  tags = ENV["tags"] || "[]"
	  category = ENV['category'] || ""
	  #--------------------------------------------------------------------------------------
	  slug = Hz2py.do(title.encode('utf-8'), :join_with => '-', :to_simplified => true)
	  slug = slug.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
	  #--------------------------------------------------------------------------------------
	  begin
	    date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d')
	  rescue Exception => e
	    puts "Error - date format must be YYYY-MM-DD, please check you typed it correctly!"
	    exit -1
	  end
	  filename = File.join(CONFIG['posts'], "#{date}-#{slug}.#{CONFIG['post_ext']}")
	  if File.exist?(filename)
	    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
	  end

	  puts "Creating new post: #{filename}"
	  open(filename, 'w') do |post|
	    post.puts "---"
	    post.puts "layout: post"
	    post.puts "title: \"#{title.gsub(/-/,' ')}\""
	    post.puts 'description: ""'
	    post.puts "category: "
	    post.puts "tags: []"
	    post.puts "---"
	    post.puts ""
	  end
	end # task :post

然后修改D:\Ruby193\lib\ruby\gems\1.9.1\gems\jekyll-1.5.1\lib\jekyll\converted.rb中36行修改为  

	def read_yaml(base, name, opts = {:encoding => "utf-8"})

还有_config.yml第一行加# encoding: utf-8   
另外D:\Ruby193\lib\ruby\gems\1.9.1\gems\hz2py-1.0.0\lib\elvuel\traditional_and_simplified.rb加两行为  

	      def conv_t2s(str)
	      	str=str.encode("UTF-8")
	        str.gsub(/#{@@t2s_hash.keys.join("|")}/){ |c| @@t2s_hash[c] }
	      end
	    
	      def conv_s2t(str)
	      	str=str.encode("UTF-8")
	        # str.gsub(/#{@@t2s_hash.values.join("|")}/){ |c| @@t2s_hash.index c }
	        str.gsub(/#{@@s2t_hash.keys.join("|")}/){ |c| @@s2t_hash[c] }
	      end


至此，汉子转拼音大功告成

阮一峰的这篇blog，让我受益匪浅
http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html

官方说三分钟完事，我折腾两小时，这水平继续提高啊

如何去除页面上Disqus评论框架的Discovery box部分？ http://www.v2ex.com/t/49949
