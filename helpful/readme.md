
# settings and stuff


## .bash_profile File

```bash
# For compilers to find zlib you may need to set:
export LDFLAGS="${LDFLAGS} -L/usr/local/opt/zlib/lib"
export CPPFLAGS="${CPPFLAGS} -I/usr/local/opt/zlib/include"

# For pkg-config to find zlib you may need to set:
export PKG_CONFIG_PATH="${PKG_CONFIG_PATH} /usr/local/opt/zlib/lib/pkgconfig"

eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"


# pyenv autocomplete
_pyenv() {
  COMPREPLY=()
  local word="${COMP_WORDS[COMP_CWORD]}"

  if [ "$COMP_CWORD" -eq 1 ]; then
    COMPREPLY=( $(compgen -W "$(pyenv commands)" -- "$word") )
  else
    local words=("${COMP_WORDS[@]}")
    unset words[0]
    unset words[$COMP_CWORD]
    local completions=$(pyenv completions "${words[@]}")
    COMPREPLY=( $(compgen -W "$completions" -- "$word") )
  fi
}
complete -F _pyenv pyenv


### change prompt when pyenv activated
function updatePrompt {
    RED='\[\e[0;31m\]'
    GREEN='\[\e[0;32m\]'
    BLUE='\[\e[0;34m\]'
    RESET='\[\e[0m\]'
    [ -z "$PYENV_VIRTUALENV_ORIGINAL_PS1" ] && export PYENV_VIRTUALENV_ORIGINAL_PS1="$PS1"
    [ -z "$PYENV_VIRTUALENV_GLOBAL_NAME" ] && export PYENV_VIRTUALENV_GLOBAL_NAME="$(pyenv global)"
    VENV_NAME="$(pyenv version-name)"
    VENV_NAME="${VENV_NAME##*/}"
    GLOBAL_NAME="$PYENV_VIRTUALENV_GLOBAL_NAME"

    # non-global versions:
    COLOR="$BLUE"
    # global version:
    [ "$VENV_NAME" == "$GLOBAL_NAME" ] && COLOR="$RED"
    # virtual envs:
    [ "${VIRTUAL_ENV##*/}" == "$VENV_NAME" ] && COLOR="$GREEN"

    if [ -z "$COLOR" ]; then
        PS1="$PYENV_VIRTUALENV_ORIGINAL_PS1"
    else
        PS1="($COLOR${VENV_NAME}$RESET)$PYENV_VIRTUALENV_ORIGINAL_PS1"
    fi
    export PS1
}
export -f updatePrompt
export PROMPT_COMMAND='updatePrompt'
```

## Terminal Theme
I am using `Solarized Dark.terminal` theme from [this](https://github.com/lysyi3m/macos-terminal-themes/tree/master/schemes) Github repo. 

Installation:
download the raw file and change the extension to .terminal and open it up using the terminal. Navigate to preferences and make it default. The prompt is different due to `change prompt` section in .bash_profile file. It will show the active pyenv environment. 
