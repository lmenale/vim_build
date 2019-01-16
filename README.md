# Building Vim from source

Compiling Vim from source is actually not that difficult.
Here's what you should do:

## 1. Install all the prerequisite libraries (including Git)

### For a **Debian-like** Linux distribution like Ubuntu, type

```sh
sudo apt install libncurses5-dev libgnome2-dev libgnomeui-dev \
libgtk2.0-dev libatk1.0-dev libbonoboui2-dev \
libcairo2-dev libx11-dev libxpm-dev libxt-dev python-dev \
python3-dev ruby-dev lua5.1 liblua5.1-dev libperl-dev git
```

On Ubuntu 16.04, `liblua5.1-dev` is the lua dev package name not `lua5.1-dev`.

_(If you know what languages you'll be using, feel free to leave out_
_packages you won't need, e.g. Python2 `python-dev` or Ruby `ruby-dev`._
_This principle heavily applies to the whole page.)_


## 2. Remove vim if you have it already.

```sh
sudo apt remove vim vim-runtime gvim
```

## 3. Once everything is installed, getting the source is easy.

Note: If you are using Python, your config directory might have
a machine-specific name (e.g. `config-3.5m-x86_64-linux-gnu`).
Check in /usr/lib/python[2/3/3.5] to find yours, and change
the `python-config-dir` and/or `python3-config-dir` arguments accordingly.

On Ubuntu 16.04, Python support was not working due to enabling 
both Python2 and Python3. Read [answer by chirinosky](http://stackoverflow.com/questions/23023783/vim-compiled-with-python-support-but-cant-see-sys-version) for workaround.

Add/remove the flags below to fit your setup. For example, you can leave out
`enable-luainterp` if you don't plan on writing any Lua.

Also, if you're not using vim 8.0,
make sure to set the VIMRUNTIMEDIR variable correctly below
(for instance, with vim 8.0a, use /usr/share/vim/vim80a).
Keep in mind that some vim installations are located directly
inside /usr/share/vim; adjust to fit your system:

```sh
git clone https://github.com/vim/vim.git
cd vim
./configure --with-features=huge \
		--enable-multibyte \
		--enable-rubyinterp=yes \
		--enable-pythoninterp=yes \
		--with-python-config-dir=/usr/lib/python2.7/config \
		--enable-python3interp=yes \
		--with-python3-config-dir=/usr/lib/python3.5/config \
		--enable-perlinterp=yes \
		--enable-luainterp=yes \
		--enable-gui=gtk2 \
		--enable-cscope \
		--prefix=/usr/local

make VIMRUNTIMEDIR=/usr/local/share/vim/vim81
```

If you want to be able to easily uninstall vim use `checkinstall`.

```sh
sudo apt install checkinstall
cd ~/vim
sudo checkinstall
```

Otherwise, you can use `make` to install.

```sh
cd ~/vim
sudo make install
```

Set vim as your default editor with `update-alternatives`.

```sh
sudo update-alternatives --install /usr/bin/editor editor /usr/local/bin/vim 1
sudo update-alternatives --set editor /usr/local/bin/vim
sudo update-alternatives --install /usr/bin/vi vi /usr/local/bin/vim 1
sudo update-alternatives --set vi /usr/local/bin/vim
```

## 4. Double check that you are in fact running the new Vim binary by looking at
the output of `vim --version`.

If you have problems, double check that you `configure`d using the correct Python config
directory, as noted at the beginning of Step 3.

These `configure` and `make` calls assume a Debian-like distro where Vim's
runtime files directory is placed in `/usr/share/vim/vim80/`,
which is not Vim's default. Same thing goes for `--prefix=/usr` in the 
`configure` call. Those values may need to be different with a Linux 
distro that is not based on Debian. In such a case, try to remove the 
`--prefix` variable in the `configure` call and the `VIMRUNTIMEDIR` in the
`make` call (in other words, go with the defaults).

If you get stuck, here's some [other useful information on building Vim](http://vim.wikia.com/wiki/Building_Vim).
