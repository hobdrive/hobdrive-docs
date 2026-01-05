load '../Rakefile'

task :upload_manuals do
    upload_yc("hd-downloads", "root/user-manual-en.pdf", "user-manual-en.pdf")
    upload_yc("hd-downloads", "root/user-manual-ru.pdf", "user-manual-ru.pdf")
end

task :prep do
    sh "cp ../hobd/changelog_ru ./ru/changelog_ru.md"
    sh "cp ../hobd/changelog_en ./en/changelog_en.md"
end

