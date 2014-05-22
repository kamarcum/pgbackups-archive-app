require "pgbackups-archive"

class PgbackupsArchive::Storage
  def store
    bucket.files.create :key => @key, :body => @file, :public => false, :encrypted => true
  end
end

class RunPgbackupsArchive
  include NewRelic::Agent::Instrumentation::ControllerInstrumentation

  def call
    Heroku::Client::PgbackupsArchive.perform
  end

  add_transaction_tracer :call, category: :task
end

namespace :pgbackups do

  desc "Perform a pgbackups backup then archive to S3."
  task :archive do
    RunPgbackupsArchive.new.call
  end

end
