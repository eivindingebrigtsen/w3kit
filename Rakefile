# Global paths
PATH = 'js/'
TMP = 'tmp'
PACK = 'pack'
ZIPS = 'zip'

desc 'Start test server'
task :tests do
	sh('cd lib && java -jar jsTestDriver-1.2.1.jar --port 4224 --browser open')	
end

desc 'Run tests'
task :test do
	sh('cd lib && java -jar jsTestDriver-1.2.1.jar --tests all')	
end


desc 'javascript lint'
task :jslint do
	puts '###### JS LINT #######'
  sh 'lib/jsl --conf lib/jsl.default.conf'
end


desc 'Replace Tabs with spaces'
task :tabs do
	puts '###### TABS #######'
  Dir[PATH+'**/*'].each do |f|
    next if File.directory?(f) 
    source = File.open(f, 'rb').read
    case File.extname(f)
    when '.js', '.css', '.html', '.sass'
			puts File.basename(f)
			next if File.dirname(f).match(/ext/)
      source.gsub!(/\t/, '  ')
    end
    File.open("#{File.dirname(f)}/#{File.basename(f)}", 'wb') {|io| io.write(source)}
  end
end


desc 'strip console logs'
task :strip => [:jslint, :tabs] do
	puts '###### STRIPPING CONSOLE CALLS #######'
  rm_rf(TMP)
  mkdir_p(TMP)
  Dir[PATH+'/**/*.js'].each do |f|
    next if File.directory?(f) || File.basename(f) =~ /(min\.|tests\.|pack\.|compiled\.)/
    source = File.open(f, 'rb').read
    file = File.basename(f)
		stripped = source
   	stripped = source.gsub(/console\.(time|log|error|warn|info|assert|group|groupEnd|timeEnd|profile|trace|profileEnd|dir)(.*?)[\;\n]/, '')
    File.open(TMP+'/'+file, 'w'){|io| io.write(stripped)}
  end
end

desc 'Pack all js into one file'
task :pack => [:tabs, :strip] do
	puts '###### PACKING JS #######'
  rm_rf(PATH+'/min')
  mkdir_p(PATH+'/min')
	files = []
  Dir[PATH+'/src/**/*.js'].each do |f|
		files << TMP+'/'+File.basename(f)
	end
	files.map do |file|
     puts "compressing #{file}"
     `java -jar lib/yuicompressor-2.4.2.jar #{file}`
  end
  File.open(PATH+'/min/all.pack.js', 'w'){|io| io.write(files.join("\n"))}
  rm_rf(TMP)
#	res = sh('cd lib && java -jar jsTestDriver-1.2.1.jar --config jsTestDriverPacked.conf --tests all')	
#	sh('growlnotify -m "Done packing javascripts \n Passed tests:'+res.to_s+'"')
end
