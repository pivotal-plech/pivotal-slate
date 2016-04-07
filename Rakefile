desc "Build the website from source"
task :build do
 puts "## Building website"
 status = system("middleman build")
 puts status ? "OK" : "FAILED"
end

desc "Deploy site to PWS"
task :deploy => :build do
  system("cp manifest.yml build && touch build/Staticfile && cd build && cf push && cd ../")
end
