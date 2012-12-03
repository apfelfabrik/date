require 'rubygems'
require 'uglifier'
require 'pathname'

task :default => ["build"]

task :dir do
	Dir::mkdir("build") unless File.exists?("build") 
	Dir::mkdir("minified") unless File.exists?("minified") 
	Dir::mkdir("minified/globalization") unless File.exists?("minified/globalization") 
end

desc "minify files only."
task :minify => ["dir"] do
	ugl = Uglifier.new(:copyright => false)
	files = FileList.new('src/*.js', 'src/globalization/*.js')
	files.each do |fileName|
		puts("Minifying " + fileName)
		source = File.read(fileName)
		min = ugl.compile(source)
		File.open('minified/' + fileName.sub(/src\//, ""), 'w') {|f| f.write min}
	end
end

task :concat do
	basic = ""

	license = File.read('src/license.js')
	core = File.read('minified/core.js')
	sugarpak = File.read('minified/sugarpak.js')
	parser = File.read('minified/parser.js')

	basic = "\n" + core + "\n" + sugarpak + "\n" + parser

	files = FileList.new('minified/globalization/*.js')
	files.each do |fileName|
		puts("Creating date-" + Pathname.new(fileName).basename)
		source = File.read(fileName)
		result = license + source + basic;
		File.open('build/date-' + Pathname.new(fileName).basename, 'w') {|f| f.write result}
	end
end

desc "Remove all build/minified files"
task :clean do
	files = FileList.new('minified/*.js', 'build/*.js', 'minified/globalization/*.js')
	files.each {|fileName| File.delete(fileName)}	
end

desc "Minify and concatinate files."
task :build => ["clean", "minify", "concat"] do
	puts("Build done!")
end