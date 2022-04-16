require 'rake/clean'
require 'fileutils'
require_relative 'lib/lib_src_file.rb'
require_relative 'lib/lib_src2md.rb'
require_relative 'lib/lib_gen_main_tex.rb'
require_relative 'lib/lib_add_head_tail_html.rb'

srcfiles = secfiles = Sec_files.new(FileList['src/sec*.src.md'].to_a.map{|file| Sec_file.new(file)})
secfiles.renum!
abstract = Src_file.new "src/abstract.src.md"

mdfiles = srcfiles.map {|file| "gfm/" + file.to_md}
htmlfiles = srcfiles.map {|file| "html/" + file.to_html}
texfiles = sectexfiles = secfiles.map {|file| "latex/" + file.to_tex}
abstract_tex = "latex/"+abstract.to_tex

["gfm", "html", "latex"].each{|d| Dir.mkdir(d) unless Dir.exist?(d)}

CLEAN.append(*mdfiles)
CLEAN << "Readme.md"

# tasks

task default: :md
task all: [:md, :html, :pdf]

task md: %w[Readme.md] + mdfiles

file "Readme.md" => [abstract] + secfiles do
  src2md abstract, abstract.to_md, "gfm"
  buf = [ "# GObject Tutorial for beginners\n", "\n" ]\
        + File.readlines(abstract.to_md)\
        + ["\n", "## Table of contents\n", "\n"]
  File.delete(abstract.to_md)
  secfiles.each_with_index do |secfile, i|
    h = File.open(secfile.path){|file| file.readline}.sub(/^#* */,"").chomp
    buf << "1. [#{h}](gfm/#{secfile.to_md})\n"
  end
  File.write("Readme.md", buf.join)
end

srcfiles.each_with_index do |srcfile, i|
  file mdfiles[i] => [srcfile] + srcfile.c_files do
    src2md srcfile, mdfiles[i], "gfm"
    if srcfile.instance_of? Sec_file
      if secfiles.size == 1
        nav = "Up: [Readme.md](../Readme.md)\n"
      elsif i == 0
        nav = "Up: [Readme.md](../Readme.md),  Next: [Section 2](#{secfiles[1].to_md})\n"
      elsif i == secfiles.size - 1
        nav = "Up: [Readme.md](../Readme.md),  Prev: [Section #{i}](#{secfiles[i-1].to_md})\n"
      else
        nav = "Up: [Readme.md](../Readme.md),  Prev: [Section #{i}](#{secfiles[i-1].to_md}), Next: [Section #{i+2}](#{secfiles[i+1].to_md})\n"
      end
      File.write(mdfiles[i], nav + "\n" + File.read(mdfiles[i]) + "\n" + nav)
    end
  end
end

task html: %w[html/index.html] + htmlfiles

file "html/index.html" => [abstract] + secfiles do
  abstract_md = "html/#{abstract.to_md}"
  src2md abstract, abstract_md, "html"
  buf = ["# GObject Tutorial for beginners\n", "\n"]\
        + File.readlines(abstract_md)\
        + ["\n", "## Table of contents\n", "\n"]
  File.delete(abstract_md)
  secfiles.each_with_index do |secfile, i|
    h = File.open(secfile.path){|file| file.readline}.sub(/^#* */,"").chomp
    buf << "1. [#{h}](#{secfile.to_html})\n"
  end
  File.write("html/index.md", buf.join)
  sh "pandoc -o html/index.html html/index.md"
  File.delete "html/index.md"
  add_head_tail_html "html/index.html"
end

srcfiles.each_with_index do |srcfile, i|
  file htmlfiles[i] => [srcfile] + srcfile.c_files do
    html_md = "html/" + srcfile.to_md
    src2md srcfile, html_md, "html"
    if srcfile.instance_of? Sec_file
      if secfiles.size == 1
        nav = "Up: [index.html](index.html)\n"
      elsif i == 0
        nav = "Up: [index.html](index.html),  Next: [Section 2](#{secfiles[1].to_html})\n"
      elsif i == secfiles.size - 1
        nav = "Up: [index.html](index.html),  Prev: [Section #{i}](#{secfiles[i-1].to_html})\n"
      else
        nav = "Up: [index.html](index.html),  Prev: [Section #{i}](#{secfiles[i-1].to_html}), Next: [Section #{i+2}](#{secfiles[i+1].to_html})\n"
      end
      File.write(html_md, nav + "\n" + File.read(html_md) + "\n" + nav)
    end
    sh "pandoc -o #{htmlfiles[i]} #{html_md}"
    File.delete(html_md)
    add_head_tail_html htmlfiles[i]
  end
end

task pdf: %w[latex/main.tex] do
  sh "cd latex; lualatex main.tex"
  sh "cd latex; lualatex main.tex"
  sh "mv latex/main.pdf latex/gobject_tutorial.pdf"
end

file "latex/main.tex" => [abstract_tex]+texfiles do
  gen_main_tex "latex", abstract.to_tex, sectexfiles
end

file abstract_tex => abstract do
  abstract_md = "latex/"+abstract.to_md
  src2md abstract, abstract_md, "latex"
  sh "pandoc --listings -o #{abstract_tex} #{abstract_md}"
  File.delete(abstract_md)
end

srcfiles.each_with_index do |srcfile, i|
  file texfiles[i] => [srcfile] + srcfile.c_files do
    tex_md = "latex/" + srcfile.to_md
    src2md srcfile, tex_md, "latex"
    if srcfile == "src/Readme_for_developers.src.md"
      sh "pandoc -o #{texfiles[i]} #{tex_md}"
    else
      sh "pandoc --listings -o #{texfiles[i]} #{tex_md}"
    end
    File.delete(tex_md)
  end
end

task :clean
task :cleanhtml do
  remove_entry_secure('html') if Dir.exist?('html')
end
task :cleanlatex do
  remove_entry_secure('latex') if Dir.exist?('latex')
end
task cleanall: [:clean, :cleanhtml, :cleanlatex]
