
require 'hoe'
require './lib/beerdb/labels/version.rb'


Hoe.spec 'beerdb-labels' do
  
  self.version = BeerDb::Labels::VERSION
  
  self.summary = 'beerdb-labels gem - beer labels (24x24, 32x32, 48x48, 64x64) bundled for reuse w/ asset pipeline'
  self.description = summary

  self.urls    = ['https://github.com/geraldb/beer.db.labels']
  
  self.author  = 'Gerald Bauer'
  self.email   = 'beerdb@googlegroups.com'
    
  self.readme_file  = 'README.md'
  self.history_file = 'History.md'
  
end


################################
#


# ls *.jpg | xargs -r -I FILE convert FILE -thumbnail 64x64 FILE_thumb.png

BEER_LABEL_SIZES = [24,32,48,64]  # e.g. 24x24, 32x32, etc.
BEER_LABEL_INPUT_DIR = 'originals'
BEER_LABEL_OUTPUT_DIR = 'vendor/assets/images/labels'



desc 'beerdb-labels - build thumbs'
task :build_thumbs  do

  files = Dir[ "#{BEER_LABEL_INPUT_DIR}/*.jpg" ]

  files.each do |filename|
    basename = File.basename( filename, '.jpg' )

    puts "filename: #{filename}, basename: #{basename}"

    BEER_LABEL_SIZES.each do |size|
      # e.g. convert #{filename} -thumbnail 48x48 vendor/assets/images/labels/48x48/#{basename}.png"
      cmd = "convert #{filename} -thumbnail #{size}x#{size} #{BEER_LABEL_OUTPUT_DIR}/#{size}x#{size}/#{basename}.png"
      puts "  #{cmd}"
      system( cmd )
    end
  end
  
  ## todo: generate lookup list of all available labels (lets us check if label exists)
  puts 'Done.'
end


desc 'beerdb-labels - build manifest'
task :build_manifest  do

  txt = File.read( 'Manifest.txt.tpl' )

  ## append all thumbnails (24x24,32x32,48x48,64x64)

  files = Dir[ "#{BEER_LABEL_INPUT_DIR}/*.jpg" ]
  files.sort!

  BEER_LABEL_SIZES.each do |size|
    files.each do |filename|
       basename = File.basename( filename, '.jpg' )

       # puts "filename: #{filename}, basename: #{basename}"
       txt << "#{BEER_LABEL_OUTPUT_DIR}/#{size}x#{size}/#{basename}.png\n"
    end
  end

  File.open( 'Manifest.txt', 'w') do |file|
    file.write( txt )
  end

  puts 'Done.'
end


desc 'beerdb-labels - build thumbnails from originals'
task :build => [:build_thumbs, :build_manifest]