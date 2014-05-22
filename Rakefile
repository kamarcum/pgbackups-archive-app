require "pgbackups-archive"


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
