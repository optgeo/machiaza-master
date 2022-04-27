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

task :csv do
  print "id,pref,city,name\n"
  1.upto(47) {|i|
    first = true
    pref = sprintf('%02d', i)
    path = "#{SRC_DIR}/mt_town_pref#{pref}.csv"
    File.foreach(path) {|l|
      if first
        first = false
      else
        r = l.strip.split(',')
        print <<-EOS
#{r[0] + r[1]},#{r[3]},#{r[6] + r[9] + r[12]},#{r[15] + r[18] + r[21]}
        EOS
      end
    }
  }
end

task :tiles do
  sh <<-EOS
rake csv > match.csv; \
rake pos | tippecanoe -f --maximum-zoom=14 --base-zoom=14 \
-r1.2 --drop-densest-as-needed --output=a.mbtiles; \
tile-join -f --output=b.mbtiles --maximum-zoom=11 a.mbtiles; \
tile-join -f --output=c.mbtiles --minimum-zoom=12 --csv=match.csv a.mbtiles; \
tile-join -f --no-tile-size-limit --no-tile-compression \
--output-to-directory=docs/zxy b.mbtiles c.mbtiles; \
rm match.csv a.mbtiles b.mbtiles c.mbtiles
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

