# 显示完整文件、文件夹路径
`defaults write com.apple.finder _FXShowPosixPathInTitle -bool TRUE;killall Finder`

# 还原显示完整文件、文件夹路径
`defaults delete com.apple.finder _FXShowPosixPathInTitle;killall Finder`

# 显示所有文件
`defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder`

# 不显示隐藏文件
`defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder`