# Linux Setup

## Intro

En esta guia se describe la configuración del sistema Linux para adaptarlo a un ambiente de trabajo para realizar
tareas relacionadas a la administración de:

* Linux Personal
* Servidores Linux
* Recursos Cloud
* Clusters Kubernetes
* Desarrollo de software

Intentare usar en su mayoría software libre, es decir, software GNU, el kernel Linux y en especificó la distro
Kubuntu en una arquitectura x86 de 64-bit.

Para esto necesito dejar algunas cosas bien configuradas:

* Entornos de desarrollo
* Gestores de paquetes
* Editor de textos
* Emulador de Terminal
* Entorno del shell
* Cliente VPN
* Navegador Web

## Objetivos

Realizaremos las siguientes actividades orientadas a instalar y configurar:

* Herramientas de linea de comando GNU
* Gestor de paquetes Apt
* Editor de textos vim
* Emulador de terminal Terminator
* El shell zsh con oh-my-zsh
* Herramientas de búsqueda en linea de comandos
* Conjunto de herramientas de linea de comandos
* Herramientas de gestión Cloud

## Gestor de paquetes APT

El programa `apt` es la herramienta de gestión de paquetes en `Kubuntu`, ya que esta basado en la distribución
`Ubuntu` y esta a su vez se basa en `Debian`.

Apt ya viene pre instalado en Kubuntu por lo que no hay que instalarlo por separado.

Lo primero que debemos aprender a usar es actualizar la lista de paquetes de los repositorios:

```shell
$ sudo apt update
```

Ahora actualizamos los paquetes del sistema:

```shell
$ sudo apt upgrade
```

**NOTA:** Estas dos tareas las debemos de ejecutar frecuentemente para mantener nuestro sistema actualizado.

Buscando paquetes:

```shell
$ sudo apt search git
```

```shell
$ sudo apt install git
```

Desinstalando paquetes:

```shell
$ sudo apt remove git
```

Actualizando paquetes:

```shell
$ sudo apt update; sudo apt upgrade
```

Limpieza de paquetes viejos:

```shell
$ sudo apt clean; sudo apt autoremove
```

## Instalando comandos GNU

Instalaremos los programas `wget` y `curl` los cuales usaremos para descargar cosas desde
internet usando la línea de comandos:

```shell
$ sudo apt install wget curl
```

## Editor de textos vim

Instalación de vim:

```shell
$ sudo apt install vim
```

Editar config:

```shell
$ vim ~/.vimrc
```

Agregamos el contenido inicial:

```shell
" .vimrc

" Configuración general
set title " Muestra el nombre del archivo en la ventana de la terminal
set number " Muestra los números de las líneas
set nowrap " No dividir la línea si es muy larga
set cursorline " Resalta la línea actual
set colorcolumn=120 " Muestra la columna límite a 120 caracteres
set nocompatible " Desactiva modo compatible
filetype plugin on " Habilita plugin para tipos de archivos
syntax on " Activa resaltado de sintaxis

" Indentación a 2 espacios
set tabstop=2
set shiftwidth=2
set softtabstop=2
set shiftround
set expandtab " Insertar espacios en lugar de <Tab>s
set imrmguicolors " Activa true colors en la terminal

" Configuracion spell check
"set spell
set nospell
setlocal spell spelllang=es,en " Corregir palabras usando diccionarios en español

" Mapeos
"inoremap " ""<left>
"inoremap ' ''<left>
"inoremap ( ()<left>
"inoremap [ []<left>
"inoremap { {}<left>
"inoremap {<CR> {<CR>}<ESC>O
"inoremap {;<CR> {<CR>};<ESC>O

" Syntax for markdown enabled
au BufNewFile,BufFilePre,BufRead *.md set filetype=markdown
au BufNewFile,BufFilePre,BufRead *.txt set filetype=markdown
set syntax=markdown

"
" PLUGINS: https://github.com/junegunn/vim-plug
"

" Specify a directory for plugins
call plug#begin('~/.vim/plugged')
" Challenger-deep-theme: https://github.com/junegunn/vim-plug
Plug 'challenger-deep-theme/vim', { 'as': 'challenger-deep' }
" NERDTree
Plug 'preservim/nerdtree'
" vimwiki
Plug 'vimwiki/vimwiki'
" editorconfig
Plug 'editorconfig/editorconfig-vim'
" Initialize plugin system
call plug#end()

"
" THEME:
"
colorscheme challenger_deep

```

Creamos directorio para spell check:

```shell
$ mkdir -p ~/.vim/spell
$ cd ~/.vim/spell
```

Descargaremos los siguientes archivos:

* es.latin1.spl
* es.latin1.sug
* es.utf-8.spl
* es.utf-8.sug

Descargamos los archivos:

```shell
$ wget https://ftp.vim.org/vim/runtime/spell/es.latin1.spl
$ wget -k https://ftp.vim.org/vim/runtime/spell/es.latin1.spl
$ wget --no-check-certificate https://ftp.vim.org/vim/runtime/spell/es.latin1.spl
$ wget https://ftp.vim.org/vim/runtime/spell/es.latin1.sug
$ wget --no-check-certificate https://ftp.vim.org/vim/runtime/spell/es.latin1.sug
$ wget --no-check-certificate https://ftp.vim.org/vim/runtime/spell/es.utf-8.spl
$ wget --no-check-certificate https://ftp.vim.org/vim/runtime/spell/es.utf-8.sug
```

Creamos los archivos para los diccionarios locales:

```shell
$ touch ~/.vim/spell/es.utf-8.add
$ touch ~/.vim/spell/en.utf-8.add
```

Shortcuts:

* **]s** – Siguiente falta ortográfica
* **[s** – Anterior falta ortográfica
* **z=** – Mostrar sugerencias para una palabra incorrecta.
* **zg** – Añadir una palabra al diccionario.
* **zug** – Deshacer la adición de una palabra al diccionario.
* **zw** – Eliminar una palabra del diccionario.

Instalar herramienta para gestión de plugins: plug-vim:

```shell
$ curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

Configuración de Plugins:

```shell
"
" PLUGINS: https://github.com/junegunn/vim-plug
"

" Specify a directory for plugins
call plug#begin('~/.vim/plugged')
" Challenger-deep-theme: https://github.com/junegunn/vim-plug
Plug 'challenger-deep-theme/vim', { 'as': 'challenger-deep' }
" NERDTree
Plug 'preservim/nerdtree'
" vimwiki
Plug 'vimwiki/vimwiki'
" Initialize plugin system
call plug#end()

"
" THEME:
"
colorscheme challenger_deep

```

Guardar vim, salir, y volver a entrar, entonces:

```shell
:PlugInstall
```

## Emulador de terminal Terminator

Instalamos el paquete Terminator:

```shell
$ sudo apt install terminator
```

Shortcuts: TODO.

## El shell zsh con oh-my-zsh

Instalamos el shell zsh:

```shell
$ sudo apt install zsh
```

Instalamos oh my zsh:

```shell
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Cambiamos el shell default:

```shell
$ chsh -s $(which zsh)
```

### Configuración de plugins integrados en zsh

Editamos el archivo de configuración de zsh:

```shell
$ vim .zshrc
```

Para habilitar los diferentes plugins cambiamos:

```shell
plugins=(git)
```

por:

```shell
plugins=(cp colored-man-pages colorize pip python git vi-mode)
```

Guardamos y re cargamos configuración:

```shell
$ source .zshrc
```

### Configuración del tema integrado en zsh

Editamos el archivo de configuración de zsh:

```shell
$ vim .zshrc
```

Para cambiar el tema cambiamos:

```shell
ZSH_THEME="robbyrussell"
```

Por:

```shell
ZSH_THEME="agnoster"
```

Guardamos y re cargamos configuración:

```shell
$ source .zshrc
```

Lista de temas: https://github.com/ohmyzsh/ohmyzsh/wiki/themes.

### Configurando tema powerlevel19k

Instalamos el tema powerlevel9k:

```shell
$ git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
```

Editamos el archivo de configuración de zsh:

```shell
$ vim .zshrc
```

Agregamos la lista de sources:

```shell
ZSH_THEME="powerlevel9k/powerlevel9k"
# Powerlevel9k settings
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(user host dir vcs)
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(status battery time)
POWERLEVEL9K_PROMPT_ON_NEWLINE=true
POWERLEVEL9K_SHORTEN_DIR_LENGTH=2
POWERLEVEL9K_OS_ICON_BACKGROUND="white"
POWERLEVEL9K_OS_ICON_FOREGROUND="blue"
POWERLEVEL9K_TIME_FORMAT="%D{%H:%M:%S | %d.%m.%y}"

```

Otras opciones:

```shell
POWERLEVEL9K_MULTILINE_FIRST_PROMPT_PREFIX="↱"
POWERLEVEL9K_MULTILINE_LAST_PROMPT_PREFIX="↳ "
```

Guardamos y re cargamos configuración:

```shell
$ source .zshrc
```

Proyecto: https://github.com/Powerlevel9k/powerlevel9k/wiki/Install-Instructions#option-2-install-for-oh-my-zsh
