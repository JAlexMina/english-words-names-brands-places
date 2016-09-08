require 'net/http'
require 'zip'
require 'tempfile'
USPTO_CSV = FileList.new("data/uspto.gov/2015/tm_assignee.csv", "data/uspto.gov/2015/tm_assignment.csv", "data/uspto.gov/2015/tm_assignor.csv")

task "uspto" => USPTO_CSV

USPTO_CSV.each do |csv|
   rule csv do |t|
      FileUtils.mkdir_p t.name.pathmap("%d")
      uri = URI ('https://bulkdata.uspto.gov/data3/trademark/assignment/economics/2015/' + t.name.pathmap("%f") + '.zip')
      download_and_extract(uri, 'data/uspto.gov/2015/')

   end
end

def download_and_extract(uri, destination)
   zipped = Net::HTTP.get(uri)
   Tempfile.open('tmp.zip') do |file|
      file.write(zipped)
      zip_file = Zip::File.open(file.path)
      zip_file.each do |file|
        file.extract destination + file.name
      end
      file.close
      file.unlink   # deletes the temp file
   end
end
