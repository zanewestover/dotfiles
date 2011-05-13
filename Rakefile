require 'erb'

task :default do
    puts 'Usage: rake [ install | force ]'
    puts '    install : ask first before overwriting'
    puts '    force   : just do it, no questions'
end

task :install do
    $replace_all = false
    installer
end

task :force do
    $replace_all = true
    installer
end

def installer
    ignore_files = %w(README.md Rakefile ssh gitconfig.erb)
    Dir['*'].each do |file|
        next if ignore_files.include?(file) or file =~ /.*~$/
        determine_action(file)
    end
    determine_action('ssh/config')
    gitconfig_installer
end

def determine_action(file)
    if File.exist?(File.join(ENV['HOME'], ".#{file}"))
        if $replace_all
            replace_file(file)
        else
            print "Overwrite ~/.#{file}? [yNaq] "
            case STDIN.gets.chomp
            when 'a'
                $replace_all = true
                replace_file(file)
            when 'y'
                replace_file(file)
            when 'q'
                exit
            else
                puts "    skipping ~/.#{file}"
            end
        end
    else
        link_file(file)
    end
end

def link_file(file)
    puts "    linking ~/.#{file}..."
    system `ln -s "$PWD/#{file}" "$HOME/.#{file}"`
end

def replace_file(file)
    puts "    removing old ~/.#{file}..."
    system `rm -f "$HOME/.#{file}"`
    link_file(file)
end

# The ~/.gitconfig file is generated dynamically so that I can have my
# GitHub API token configured in it without putting my token into my GitHub
# dotfiles repo, which is publicly visible.

def gitconfig_installer
    template_file = 'gitconfig.erb'
    output_file = ENV['HOME'] + '/.gitconfig'

    # If we find an older install with the symlink in place,
    # clean that up first
    if File.symlink?(output_file)
        File.unlink(output_file)
        puts "    deleted symlink #{output_file}..."
    end

    puts ''
    puts "=== Creating #{output_file} ==="
    puts ''

    print '    Name: '
    $git_name = STDIN.gets.chomp

    print '    Email address: '
    $git_email = STDIN.gets.chomp

    print '    GitHub username: '
    $github_username = STDIN.gets.chomp

    print '    GitHub API token: '
    $github_api_token = STDIN.gets.chomp

    template = ERB.new(File.read(template_file))
    File.open(output_file, 'w') do |f|
        f.write(template.result())
    end

    ret = File.chmod(0600, output_file)
    if ret != 1
        puts "WARN: chmod of #{output_file} failed!"
    end

end

