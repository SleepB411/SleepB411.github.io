---
title: "Powershell Profile Config 01"
author: "dog_egg"
date: 2024-06-07 01:00:00 -0400
categories: [Blogging, Dotfile]
tags: [powershell, config]
description: "Powershell Configuration Log"
toc: false # false to disable the table of contents ont the right panel of the post, can turn it off globally in _config.yml
---

### Prerequisites
- Download poweshell from miscrosoft store 
 
> Not the windows power shell but the new one from the store
{: .prompt-warning }

- `Windows Terminal` Terminal
  - There are so many fancy features in the new terminal 
  - Acrylic
  - Customizable theme `json` file
  - Retro terminal effect 
  - etc.
   
- `scoop` Package manager
   ```sh
      scoop install oh-my-posh    
    ```
    - Refer this article for customizing oh-my-posh theme [oh-my-posh](https://ohmyposh.dev/docs/installation/customize)

   ```sh
      scoop install fzf
    ```
- `neovim`, `neorg` plugin for neovim might be needed for `Aliases-2` functions

### Powershell Profile
  - Create and Edit `Microsoft.PowerShell_profile.ps1`
    - Normally, can be found using the following command
    ```sh
      cd $PROFILE
    ```
  - Create and Edit `user_profile.ps1` under the directory `C:\Users\username\.config\powershell`
  - Add the directory to env variable `USERPROFILE` 
  - Add the following line in the `Microsoft.Powershell_profile.ps1` file
  ```powershell
    $env:USERPROFILE\.config\powershell\user_profile.ps1
  ```
  - THE `user_profile.ps1` FILE
```powershell
# disable python venv prompt
$env:VIRTUAL_ENV_DISABLE_PROMPT = 1


# TODO: follow instruction on the site to customize the theme
# powershell Theme  (NOTE: Self customized one, refer this website for other themes https://ohmyposh.dev/docs/installation/customize)
oh-my-posh init pwsh --config 'solarized_dog_egg.omp.json' | Invoke-Expression 

# Icons
Import-Module -Name Terminal-Icons # Windows Terminal Icons for better `ls` experience
# TODO: find the correct path for ChocolateyInstall
Import-Module $env:ChocolateyInstall\helpers\chocolateyProfile.psm1 # For 

# PSReadLine
# My Common Usage: 
# 1. Ctrl + k: delete from the curor to the end of the line
# 2. Ctrl + u: delete from the cursor to the beginning of the line
Set-PSReadLineOption -EditMode Emacs
Set-PSReadLineOption -BellStyle None
Set-PSReadLineKeyHandler -Chord 'Ctrl+d' -Function DeleteChar
Set-PSReadLineOption -PredictionSource History

# Fzf
# Ctrl + r: command history
# Ctrl + f: fuzzy find files
Import-Module PSFzf
Set-PsFzfOption -PSReadlineChordProvider 'Ctrl+f' -PSReadlineChordReverseHistory 'Ctrl+r'


# Aliases-1
Set-Alias vim nvim 
Set-Alias g git
Set-Alias grep findstr # e.g. ls | grep "pattern"
# TODO: change directory to git
Set-Alias less 'DirectoryToGit\usr\bin\less.exe' # e.g. cat filename | less 
Set-Alias lg lazygit 
Set-Alias open Invoke-Item # open current director in file explorer
Set-Alias rn ren # rename

 
# TODO: cusotmize the functions except `which`
# Aliases-2
Set-Alias journal Get-Journal # I use neorg for journaling, which is a plugin for neovim (need customization i nthe function for journal directory)
Set-Alias aienv Get-VEnv # activate my most frequently used python venv
Set-Alias blog Write-Blog # write blog post (need customization in the function for blog directory)

# Utilities
# which command behave like the linux command
function which ($command) {
  Get-Command -Name $command -ErrorAction SilentlyContinue |
    Select-Object -ExpandProperty Path -ErrorAction SilentlyContinue
}


# Journaling function (need neorg plugin for neovim)
function Get-Journal{
  $directory = "$HOME/Super/journal"
  if (-not (Test-Path $directory)) {
    Write-Error "Go to user-profile.ps1 to double check the directory in function {Get-Journal}!"
  }
  cd "$HOME/Super"
  nvim "$HOME/Super/journal/index.norg"
}

# Python venv function
function Get-VEnv{
  conda activate pykan
}

# Blogging function
function Write-Blog{
  param (
    [string]$Argument
  )
  $directory = "$HOME/Super/myblog/_posts"
  if (-not (Test-Path $directory)) {
    Write-Error "Go to user-profile.ps1 to double check the directory in function {Write-Blog}!"
  }
  cd "$HOME/Super/myblog/_posts"
  nvim "$HOME/Super/myblog/_posts/2024-06-07-$Argument.md"
}
```

