# zsh 

Check the current shell used  

    echo $SHELL

## Install ZSH

    yay -S zsh
## Install Plugins

    yay -S zsh-autosuggestions
    yay -S zsh-syntax-highlighting
### zsh-safe-paste
    sudo wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/plugins/safe-paste/safe-paste.plugin.zsh -O /usr/share/zsh/plugins/safe-paste.plugin.zsh
    sudo chmod 644 /usr/share/zsh/plugins/safe-paste.plugin.zsh
### FZF
Search for files:       `ctrl + t`

Search for commands:    `ctrl + r`

> Setup

    git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf

> Exec 

    ~/.fzf/install
    
 ### sudo 

To use: double tap on `esc`

    sudo wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/plugins/sudo/sudo.plugin.zsh -O /usr/share/zsh/plugins/sudo.plugin.zsh  
    
### Colorls
> yay

    yay ruby-colorls

> Icon 
    cd /tmp 
    yay -S ttf-meslo-nerd-font-powerlevel10k


>  needed ??? 
    curl -L 'https://github.com/ryanoasis/nerd-fonts/blob/master/src/glyphs/PowerlineSymbols.otf?raw=true' --output PowerlineSymbols.otf
    
    curl -L 'https://github.com/ryanoasis/nerd-fonts/blob/master/src/glyphs/PowerlineExtraSymbols.otf?raw=true' --output PowerlineExtraSymbols.otf
    curl -L 'https://github.com/ryanoasis/nerd-fonts/blob/master/src/glyphs/font-awesome-extension.ttf?raw=true' --output font-awesome-extension.otf

    curl -L 'https://github.com/ryanoasis/nerd-fonts/blob/master/src/glyphs/FontAwesome.otf?raw=true' --output FontAwesome.otf




     

## zshrc Config

nano ~/.zshrc
```zsh
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source /usr/share/zsh/plugins/safe-paste.plugin.zsh
source /usr/share/zsh/plugins/DynamicTitle.zsh
source /usr/share/zsh/plugins/bindkey.zsh
source /usr/share/zsh/plugins/sudo.plugin.zsh
source /usr/share/zsh/plugins/prompt.zsh
#source $(dirname $(gem which colorls))/tab_complete.sh
#source /usr/lib/ruby/gems/2.7.0/gems/colorls-1.3.2/lib/tab_complete.sh
source ~/.fzf.zsh

RPROMPT='%D{[%K:%M:%S]}'

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


# For directory listing 
LS_COLORS='rs=0:di=01;34:ln=01;36:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:su=37;41:sg=30;43:tw=30;42:ow=34;42:st=37;44:ex=01;32:';
export LS_COLORS

# For directory listing 
alias l='colorls --group-directories-first'
alias ll='colorls --group-directories-first --long' # detailed 
alias lh='colorls --group-directories-first --almost-all'
alias llh='colorls --group-directories-first --almost-all --long' # detailed 
alias ls='ls --color=auto --human-readable'
alias tree='colorls --tree'

# Grep colours
alias grep='grep --colour=auto'
alias egrep='egrep --colour=auto'

# Custom command
alias smbmap='smbmap --no-banner'
alias gobusterd='gobuster dir -x .php,.html -e -s 200,204,301,302,307'
alias evil-winrm='evil-winrm -s "/home/bob/CTF/HTB/WindowsTools/powershell"'
alias powershell='pwsh'
alias spm='sudo pacman'
alias netstat='netstat --numeric-ports'

# Command history
HISTSIZE=1000
SAVEHIST=1000
HISTFILE=~/.history

setopt extended_history         # record timestamp of command in HISTFILE
setopt hist_ignore_space        # ignore commands that start with space
setopt hist_verify              # show command with history expansion to user before running it
setopt hist_ignore_all_dups     # prevent history from recording duplicated entries

# Command correction
setopt correctall

# Advanced Tab completion
autoload -U compinit
compinit
```

### Prompt
sudo nano /usr/share/zsh/plugins/prompt.zsh

```zsh
netInfo=$(ifconfig)
netInfoTun0=$(echo $netInfo | grep -A 1 'tun0')

if [[ $netInfoTun0 ]]; then 
    ip=$(echo $netInfoTun0 | tail -1 | cut -d ' ' -f 10)
    interfaceName="tun0"
else
    netInfoEns33=$(echo $netInfo | grep -A 1 'ens33')
    if [[ $netInfoEns33 ]]; then 
        ip=$(echo $netInfoEns33 | tail -1 | cut -d ' ' -f 10)
        interfaceName="ens33"
    fi
fi
RPROMPT="[$interfaceName:%F{red}$ip%f]~%D{[%K:%M:%S]}"


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
```


### Dynamic Title

sudo nano /usr/share/zsh/plugins/DynamicTitle.zsh

```zsh
autoload -Uz add-zsh-hook

function xterm_title_precmd () {
    print -Pn -- '\e]2;%n@%m %~\a'
    [[ "$TERM" == 'screen'* ]] && print -Pn -- '\e_\005{g}%n\005{-}@\005{m}%m\005{-} \005{B}%~\005{-}\e\\'
}

function xterm_title_preexec () {
    print -Pn -- '\e]2;%n@%m %~ %# ' && print -n -- "${(q)1}\a"
    [[ "$TERM" == 'screen'* ]] && { print -Pn -- '\e_\005{g}%n\005{-}@\005{m}%m\005{-} \005{B}%~\005{-} %# ' && print -n -- "${(q)1}\e\\"; }
}

if [[ "$TERM" == (screen*|xterm*|rxvt*|tmux*|putty*|konsole*|gnome*) ]]; then
    add-zsh-hook -Uz precmd xterm_title_precmd
    add-zsh-hook -Uz preexec xterm_title_preexec
fi
```

### Bindkey

sudo nano /usr/share/zsh/plugins/bindkey.zsh

```zsh
# create a zkbd compatible hash;
# to add other keys to this hash, see: man 5 terminfo
typeset -g -A key

key[Home]="${terminfo[khome]}"
key[End]="${terminfo[kend]}"
key[Insert]="${terminfo[kich1]}"
key[Backspace]="${terminfo[kbs]}"
key[Delete]="${terminfo[kdch1]}"
key[Up]="${terminfo[kcuu1]}"
key[Down]="${terminfo[kcud1]}"
key[Left]="${terminfo[kcub1]}"
key[Right]="${terminfo[kcuf1]}"
key[PageUp]="${terminfo[kpp]}"
key[PageDown]="${terminfo[knp]}"
key[ShiftTab]="${terminfo[kcbt]}"

# setup key accordingly
[[ -n "${key[Home]}"      ]] && bindkey -- "${key[Home]}"      beginning-of-line
[[ -n "${key[End]}"       ]] && bindkey -- "${key[End]}"       end-of-line
[[ -n "${key[Insert]}"    ]] && bindkey -- "${key[Insert]}"    overwrite-mode
[[ -n "${key[Backspace]}" ]] && bindkey -- "${key[Backspace]}" backward-delete-char
[[ -n "${key[Delete]}"    ]] && bindkey -- "${key[Delete]}"    delete-char
[[ -n "${key[Up]}"        ]] && bindkey -- "${key[Up]}"        up-line-or-history
[[ -n "${key[Down]}"      ]] && bindkey -- "${key[Down]}"      down-line-or-history
[[ -n "${key[Left]}"      ]] && bindkey -- "${key[Left]}"      backward-char
[[ -n "${key[Right]}"     ]] && bindkey -- "${key[Right]}"     forward-char
[[ -n "${key[PageUp]}"    ]] && bindkey -- "${key[PageUp]}"    beginning-of-buffer-or-history
[[ -n "${key[PageDown]}"  ]] && bindkey -- "${key[PageDown]}"  end-of-buffer-or-history
[[ -n "${key[ShiftTab]}"  ]] && bindkey -- "${key[ShiftTab]}"  reverse-menu-complete

# Finally, make sure the terminal is in application mode, when zle is
# active. Only then are the values from $terminfo valid.
if (( ${+terminfo[smkx]} && ${+terminfo[rmkx]} )); then
    autoload -Uz add-zle-hook-widget
    function zle_application_mode_start {
        echoti smkx
    }
    function zle_application_mode_stop {
        echoti rmkx
    }
    add-zle-hook-widget -Uz zle-line-init zle_application_mode_start
    add-zle-hook-widget -Uz zle-line-finish zle_application_mode_stop
fi

bindkey '^[[1;5C' emacs-forward-word        #control left
bindkey '^[[1;5D' emacs-backward-word       #control right
```

## Set as default 
`chsh -s $(which zsh)`
