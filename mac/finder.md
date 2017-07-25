# 显示完整文件、文件夹路径
`defaults write com.apple.finder _FXShowPosixPathInTitle -bool TRUE;killall Finder`

# 还原显示完整文件、文件夹路径
`defaults delete com.apple.finder _FXShowPosixPathInTitle;killall Finder`
