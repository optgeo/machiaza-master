require './constants.rb'

task :download do
  1.upto(47) {|i|
    pref = sprintf('%02d', i)
    URLS.each {|url|
      url = url.sub("${pref}", pref)
      src_path = "#{SRC_DIR}/#{pref}.zip"
      sh "curl -o #{src_path} #{url}"
      sh "unzip -d #{SRC_DIR} #{src_path}"
      sh "rm #{src_path}"
    }
  }
end

task :clean do
  sh "rm -rf #{SRC_DIR}"
end

