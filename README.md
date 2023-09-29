# Linux Setup

## Introducción

En esta guia se describe la configuración del sistema Linux para adaptarlo a un ambiente de trabajo para realizar
tareas relacionadas a la administración de:

* Linux Personal
* Servidores Linux

Intentare usar en su mayoría software libre, es decir, software GNU, el kernel Linux y en especificó la distro
Kubuntu en una arquitectura x86 de 64-bit.

### Objetivos

Realizaremos las siguientes actividades:

* Gestión de paquetes Apt
* Instalación de herramientas GNU
* Configurar el editor de textos vim
* Configurar el shell zsh con oh-my-zsh
* Configurar el emulador de terminal kitty

## Gestión de paquetes Apt

Usaremos el programa `apt` para gestionar los paquetes del sistema, Apt ya viene pre instalado en Kubuntu por
lo que no hay que instalarlo por separado.

**NOTA:** Ejecutaremos apt con `sudo` ya que se requieren permisos de administrador para las siguientes operaciones.

Lo primero que debemos aprender a usar es actualizar la lista de paquetes de los repositorios:

```shell
sudo apt update
```

Ahora actualizamos los paquetes del sistema:

```shell
sudo apt upgrade
```

**NOTA:** Estas dos tareas las debemos de ejecutar frecuentemente para mantener nuestro sistema actualizado.

Ahora que el sistema esta actualizado, veremos como usar apt para buscar paquetes, por ejemplo, buscamos
el programa `git`:

```shell
sudo apt search git
```

Para instalar paquetes usamos:

```shell
sudo apt install git
```

Para desinstalar paquetes usamos:

```shell
sudo apt remove git
```

## Instalación de herramientas CLI

Instalaremos los programas de linea de comandos `wget` y `curl` los cuales usaremos para descargar cosas desde
internet usando la línea de comandos:

```shell
sudo apt install wget curl whois ipcalc mtr nmap
```

Instalaremos algunas herramientas útiles para el manejo de archivos:

```shell
sudo apt install tree fzf silversearcher-ag
```

## Configurar el editor de textos vim

Instalamos el editor de textos `vim` con el siguiente comando:

```shell
sudo apt install vim
```

Editamos el archivo `~/.vimrc` para cambiar la configuración del editor:

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
```

### Usando el corrector ortográfico

Creamos directorio para spell check:

```shell
mkdir -p ~/.vim/spell
cd ~/.vim/spell
```

Descargaremos los siguientes archivos de diccionarios:

```shell
wget --no-check-certificate https://ftp.vim.org/vim/runtime/spell/es.latin1.spl
wget --no-check-certificate https://ftp.vim.org/vim/runtime/spell/es.latin1.sug
wget --no-check-certificate https://ftp.vim.org/vim/runtime/spell/es.utf-8.spl
wget --no-check-certificate https://ftp.vim.org/vim/runtime/spell/es.utf-8.sug
```

Creamos los archivos para los diccionarios locales:

```shell
touch ~/.vim/spell/es.utf-8.add
touch ~/.vim/spell/en.utf-8.add
```

Para usar el corrector ortográfico, añadimos las siguientes líneas al final del archivo `.vimrc`:

```
" Configuración spell check
"set spell
set nospell
setlocal spell spelllang=es,en " Corregir palabras usando diccionarios en español
```

Para hacer fácil el uso, aconsejo usar los siguientes atajos de teclado:

* **]s** – Siguiente falta ortográfica
* **[s** – Anterior falta ortográfica
* **z=** – Mostrar sugerencias para una palabra incorrecta.
* **zg** – Añadir una palabra al diccionario.
* **zug** – Deshacer la adición de una palabra al diccionario.
* **zw** – Eliminar una palabra del diccionario.

### Instalando plugins con plug-vim

Ahora vamos a instalar la herramienta para gestión de plugins `plug-vim`:

```shell
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

Editamos el archivo de configuración de vim:

```shell
vim .vimrc
```

Al final del archivo de configuración de vim agregar el siguiente bloque:

```shell
"
" PLUGINS: https://github.com/junegunn/vim-plug
"

" Specify a directory for plugins
call plug#begin('~/.vim/plugged')
" NERDTree
Plug 'preservim/nerdtree'
" vimwiki
Plug 'vimwiki/vimwiki'
" Initialize plugin system
call plug#end()
```

Guardar vim, salir, y volver a entrar, entonces ejecutamos el siguiente comando:

```shell
:PlugInstall
```

### Explorando archivos con NERDtree

NERDTree es un explorador del sistema de archivos para el editor Vim. Al usar este plugin, los usuarios pueden
explorar visualmente jerarquías de directorios complejas, abrir archivos rápidamente para leerlos o editarlos y
realizar operaciones del sistema de archivos básicas.

Para abrir NERDtree en vim, usa el comando `:NERDtree`, y dentro del explorador de archivos puedes usar los siguientes
atajos de teclado:

* t: Abrir el archivo seleccionado en una pestaña nueva
* i: Abrir el archivo seleccionado en una ventana dividida horizontalmente
* s: Abrir el archivo seleccionado en una ventana dividida verticalmente
* I: Muestra los archivos ocultos
* m: Muestra el menu de NERDtree
* R: Refresca el árbol, útil si los archivos cambian fuera de Vim

Para personalizar el uso añade las siguientes lineas al archivo de configuración de vim:

```
" NERDTree mapping
nnoremap <C-n> :NERDTree<CR>
nnoremap <C-t> :NERDTreeToggle<CR>
```

Con estos mapeos de teclas, podemos usar **ctrl+n** para abrir NERDtree, y **ctrl+t** para ocultarlo.

### Configurando el soporte para markdown

Agregamos al final del archivo `.vimrc` las siguientes configuraciones para trabajar con archivos markdown:

```
" Syntax for markdown enabled
au BufNewFile,BufFilePre,BufRead *.md set filetype=markdown
au BufNewFile,BufFilePre,BufRead *.txt set filetype=markdown
set syntax=markdown
```

Guardamos el archivo y probamos abriendo algún archivo con formato markdown.

### Configurando integración con fzf

Agregamos al final del archivo `.vimrc` las siguientes configuraciones para trabajar con archivos markdown:

```
" Habilitar fzf
set rtp+=/usr/bin/fzf
```

Guardamos el archivo y probamos.

## Configurar el shell zsh con oh-my-zsh

Instalamos el shell zsh:

```shell
sudo apt install zsh
```

Instalamos oh my zsh:

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Cambiamos el shell default:

```shell
chsh -s $(which zsh)
```

### Configuración de plugins integrados en zsh

Editamos el archivo de configuración de zsh:

```shell
vim .zshrc
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
source .zshrc
```

### Configuración del tema integrado en zsh

Editamos el archivo de configuración de zsh:

```shell
vim .zshrc
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
source .zshrc
```

Lista de temas: https://github.com/ohmyzsh/ohmyzsh/wiki/themes.

### Configurando tema powerlevel19k

Instalamos el tema powerlevel9k:

```shell
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
```

Editamos el archivo de configuración de zsh:

```shell
vim .zshrc
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
source .zshrc
```

Proyecto: https://github.com/Powerlevel9k/powerlevel9k/wiki/Install-Instructions#option-2-install-for-oh-my-zsh

### Configurando una fuente Powerline

Para que el tema pueda renderizar apropiadamente, necesitas una fuente que tenga los glifos *Powerline*. Estos son
usados al inicio y al final de los segmentos para producir la apariencia *powerline*. Ademas, si la fuente incluye
alguno de los conjuntos de glifos expandidos, Powerlevel9k es capaz de hacer uso de ellos para producir un prompt
muy chulo.

Descarga la última version de los símbolos de la fuente y el archivo de `fontconfig`:

```
wget https://github.com/powerline/powerline/raw/develop/font/PowerlineSymbols.otf
wget https://github.com/powerline/powerline/raw/develop/font/10-powerline-symbols.conf
```

Mueve los símbolos de la fuente a una ruta de fuentes valida.

```
mkdir ~/.fonts/
mv PowerlineSymbols.otf ~/.fonts/
```

Actualiza el cache de las fuentes para la nueva ruta donde se movieron los archivos:

```
fc-cache -vf ~/.fonts/
```

Instala el archivo de fontconfig:

```
mkdir -p .config/fontconfig/conf.d
mv 10-powerline-symbols.conf ~/.config/fontconfig/conf.d/
```

Cierra la terminal y verifica que ya aparecen los símbolos en el prompt.

## Emulador de terminal kitty

Kitty es un emulador de terminar rico en funcionalidades, basado en GPU y muy rápido.

Instalamos kitty con el script instalador:

```
curl -L https://sw.kovidgoyal.net/kitty/installer.sh | sh /dev/stdin
```

Creamos un directorio local para binarios, y hacemos un enlace simbólico de el binario de Kitty a la ruta local:

```
mkdir ~/.local/bin
ln -sf ~/.local/kitty.app/bin/kitty ~/.local/bin/
ln -sf ~/.local/kitty.app/bin/kitten ~/.local/bin/
```

Ahora editamos el archivo de inicialización del shell zsh para ajustar el PATH:

```
vim .zshrc
```

Agregamos o modificamos la linea que exporta la variable `PATH`:

```
export PATH=$HOME/bin:.local/bin:$PATH
```

Y finalmente recargamos la configuración del shell:

```
source .zshrc
```

Descargamos los temas de kitty:

```
git clone --depth 1 https://github.com/dexpota/kitty-themes.git ~/.config/kitty/kitty-themes
```

Configuramos el tema de dracula:

```
cd ~/.config/kitty
ln -s ./kitty-themes/themes/Dracula.conf ~/.config/kitty/theme.conf
```
