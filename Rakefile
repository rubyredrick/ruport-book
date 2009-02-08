task :default => [:build]

task :build do
  sh "export PATH=/opt/local/bin:$PATH"
  sh "dblatex xml/book.xml -o book.pdf"
  sh "open book.pdf"
end

task :convert do
  basefile = File.basename(ARGV[1]).sub(/\.txt/,".xml")
  sh "export PATH=/opt/local/bin:$PATH"
  sh "asciidoc -d book -b docbook -o xml/ch_#{basefile} -s #{ARGV[1]}"
end

task :convert_all do
  targetDir = File.join(File.dirname(__FILE__), 'xml')
  sh "export PATH=/opt/local/bin:$PATH"
  FileList[File.join(File.dirname(__FILE__), 'manuscript', '*.txt')].each do |src|
    unless /foreword/=~ src
      target = File.join targetDir, File.basename(src).sub(".txt", ".xml")
      sh "asciidoc -d book -b docbook -o xml/ch_#{File.basename(target)} -s #{src}"
    end
  end
end

task :fix_images do
  local_project_dir = File.dirname(__FILE__)
  FileList[File.join(local_project_dir, 'manuscript', '*.txt')].each do |src|
    output_file = src.sub(".txt", '.tmp')
    File.open(output_file, 'w') do |out|
      File.foreach(src) do |line|
        out.puts line.gsub("/Users/chenoa/devel/ruport_book", local_project_dir)
      end
    end
    mv output_file, src
  end
end
