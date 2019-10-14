# zsh 

Check the current shell used  

    echo $SHELL

## Install ZSH

    pacman -S zsh
## Install Plugins

    pacman -S zsh-autosuggestions
    pacman -S zsh-syntax-highlighting
### zsh-safe-paste
[safe-paste.plugin.zsh](https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/safe-paste/safe-paste.plugin.zsh)

    mkdir /usr/share/zsh-safe-paste
    nano /usr/share/zsh-safe-paste/safe-paste.plugin.zsh
        ...
    chmod 644 /usr/share/zsh-safe-paste/safe-paste.plugin.zsh


## zshrc Config

nano ~/.zshrc
```bash
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source /usr/share/zsh-safe-paste/safe-paste.plugin.zsh


RPROMPT='%D{[%I:%M:%S]}'

# If running as root and nice >0, renice to 0.
if [ "$USER" = 'root' ] && [ "$(cut -d ' ' -f 19 /proc/$$/stat)" -gt 0 ]; then
    renice -n 0 -p "$$" && echo "# Adjusted nice level for current shell to 0."
fi

case $USER in
    root)
        PROMPT='[%F{red}%n%F{blue}@%F{red}%m%f] %F{blue}%B%~%b%f $ '
    ;;

    *)  
        PROMPT='[%F{white}%n%F{blue}@%F{red}%m%f] %F{blue}%B%~%b%f $ '

    ;;
esac



#-----------------------------
# Dircolors
#-----------------------------
LS_COLORS='rs=0:di=01;34:ln=01;36:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:su=37;41:sg=30;43:tw=30;42:ow=34;42:st=37;44:ex=01;32:';
export LS_COLORS

alias grep='grep --colour=auto'
alias egrep='egrep --colour=auto'
alias ls="ls --color=auto --human-readable"
alias spm="sudo pacman"
alias netstat=netstat --numeric-ports

# Bindkey
bindkey '^[[1;5C' emacs-forward-word        #control left
bindkey '^[[1;5D' emacs-backward-word       #control right

# Command history 
HISTSIZE=1000
SAVEHIST=1000
HISTFILE=~/.history

setopt extended_history # record timestamp of command in HISTFILE
setopt hist_ignore_space # ignore commands that start with space
setopt hist_verify # show command with history expansion to user before running it

# Advanced Tab completion
autoload -U compinit
compinit

```

## Set as default 
`chsh -s $(which zsh)`
