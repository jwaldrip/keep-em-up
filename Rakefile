require 'bundler/setup'
require 'net/https'
require 'zlib'
require 'json'

Bundler.require :default

module Pinger
  extend self

  def ping_sites!
    uri = URI('https://api.heroku.com/apps')
    req = Net::HTTP::Get.new(uri)
    req.basic_auth 'jason@waldrip.net', 'c49e61c6fe4b76a52ce1ad2317ff5dbbdec2e16b'
    req['Accept'] = 'application/vnd.heroku+json; version=3'
    http = Net::HTTP.new(uri.hostname, uri.port)
    http.use_ssl = true
    res = http.start { |http| http.request(req) }
    apps = JSON.parse Zlib::GzipReader.new(StringIO.new res.body).read
    apps.each do |app|
      print "fetching #{app['web_url']}"
      uri = URI(app['web_url'])
      res = Net::HTTP.get_response uri
      print " #{res.code}"
      puts "\n"
    end
  end

end

task :run do
  loop do
    Pinger.ping_sites!
    sleep 60 * 5
  end
end