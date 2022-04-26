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

task :pos do
  1.upto(47) {|i|
    first = true
    pref = sprintf('%02d', i)
    path = "#{SRC_DIR}/mt_town_pos_pref#{pref}.csv"
    File.foreach(path) {|l|
      if first
        first = false
      else
        r = l.strip.split(',')
        print <<-EOS
{"type": "Feature", "geometry": {"type": "Point", "coordinates": [#{r[3]}, #{r[4]}]}, "properties": {"id": "#{r[0] + r[1]}"}, "tippecanoe": {"layer": "machiaza"}}
        EOS
      end
    }
  }
end

task :tippecanoe do
  sh <<-EOS
rake pos | tippecanoe -f --maximum-zoom=12 --base-zoom=12 \
-rg --drop-densest-as-needed \
--no-tile-compression --output-to-directory=docs/zxy
  EOS
end

task :style do
  sh <<-EOS
charites build style.yml docs/style.json
  EOS
end

task :host do
  sh <<-EOS
budo -d docs
  EOS
end

