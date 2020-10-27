Camel-vim
==========================


### Introduction ###
Camel-vim can build a vim-plugin-based environment for c/c++. It will install and configure YouCompleteMe and some other plugins.<br>
It allows you to search and replace words, comment and uncomment codes.<br>
It allows you to jump to header file, to declaration and to definition recusively.<br>
It allows you to create files with templates.<br>
It allows you to format codes with K&R style.


### Tested OS list
Ubuntu 18.04


### Installation ###
Camel-vim supports vim 7 and vim 8. The branch master is for vim 7 and the branch vim8 is for vim 8.<br>
Execute `./check.sh` to check if the environment is suitable for the installation.<br>
Execute `./install.sh` to do the installation.


##### Issues #####
a) YouCompleteMe may have many issues. If there are some failures about it, try to reinstall it and execute `./install_YCM.sh` manually. Here are two examples:<br>
- Downloading clang may fail while installing YCM. In this case, you need to download clang (`libclang-7.0.0-x86_64-unknown-linux-gnu.tar.bz2` for x86_64) manually from https://dl.bintray.com/micbou/libclang/ and put it into `~/.vim/bundle/YouCompleteMe/third_party/ycmd/clang_archives/`, then execute `./install_YCM.sh` to finish the installation.<br>
- Omnisharp for c# may fail on downloading for some reason. This error can be ignored if you don't use c#. Otherwise, you can manually download it from https://github.com/OmniSharp/omnisharp-roslyn/releases/download/v1.32.19/omnisharp.http-linux-x64.tar.gz and move it into `~/.vim/bundle/YouCompleteMe/third_party/ycmd/third_party/omnisharp-roslyn/v1.32.19/`, then execute `./install_YCM.sh` to finish the installation.<br>

b) On Mac OS, you need to install `ctags` with `brew` with the command: `brew install ctags`, and then add ``alias ctags="`brew --prefix`/bin/ctags"`` into the `~/.bashrc`.


### General Usage ###
Execute `source ~/.bashrc` after the installation to make `vimc` work.<br>
To start a C/C++ project, execute `vimc` at the root of the project.<br>
Why "at the root of project"? To ensure that searching, jumping and formatting in the project works as expected with the help of `.ycm_extra_conf.py`, `.tags` and `.clang-format` generated by executing `vimc`.<br>

##### Normal Mode
`:Q<CR>`: quit vim and all plugins, delete `.ycm_extra_conf.py`, `.clang-format` and `.tags` in the root of project<br>
`:W<CR>`: save all & `:Q<CR>`<br>
`<C-k>` : save<br>

`\tg`: open or close the window of taglist<br>

`<F5>`: go to shell, equivalent to `:term<CR>`<br>
`gg=G`: format the current file with K&R style with the tool clang-format<br>

`mh`: move cursor to left window<br>
`mj`: move cursor to bottom window<br>
`mk`: move cursor to top window<br>
`ml`: move cursor to right window<br>
`mw`: move cursor to window below/right of the current one<br>
`mt`: move cursor to top-left window<br>
`mb`: move cursor to bottom-right window<br>
`mp`: move cursor to previous window<br>

`+`: increase width of window towards left<br>
`-`: decrease width of window towards right<br>

`\cc`: comment one line<br>
`\cv`: comment one line with next delimiter<br>
`\cm`: comment multi lines<br>
`\c$`: comment to end of line<br>
`\cu`: uncomment<br>

`<C-p>`: search file in project<br>

`:Grep [keyword]<CR>`: search the keyword in project<br>
`\vv`: search the word under cursor in project<br>
`:Replace [keyword]<CR>`: replace the word under cursor with `keyword` in project<br>
`:Replace [target] [keyword]<CR>`: replace the word `target` with `keyword` in project<br>
`:UndoReplace<CR>`: undo `:Replace`<br>
`cgt`: save changes and close all tabs except the first, then close the bottom-right window<br>
`:ccl<CR>`: close `Grep` window<br>

`\'`: wrap selected part by "'", if no part is selected, the word under cursor will be wrapped<br>
`\\'`: remove the closest "'" wrapped the part in which the cursor is<br>
similar shortcuts: `\"` and `\\"`, `\(` and `\\(`, `\[` and `\\[`, `\{` and `\\{`, `\<` and `\\<`<br>

`<F12>`: goto header file<br>
`<C-]>`: goto declaration or to definition<br>
`g<C-]>`: goto the only match or list multi matches<br>
`<C-o>`: go backword<br>
`<C-i>`: go forward<br>

`:Termdebug<CR>`: run debugger by default of Vim

##### Insert Mode
`<C-j>`: move cursor backwards out of parenthesis<br>
`<C-k>`: goto normal mode and save<br>
`<C-e>`: move the current character to the end of next word<br>
`<C-l>`: move the cursor to the end of line<br>
`<C-\>`: delete the word under the cursor<br>

To get more information about usage, click on the links at the References below.


### Tips ###
0) Edit the line 59 to 61 of `ycm_extra_conf.py` to change the searching path of YCM.<br>
1) `:YcmGenerateConfig` has been executed automatically while enterning vim, it enables "goto declaration or definition" if `Makefile` or `CMakeLists.txt` is used. Re-execute it if necessary (`:YcmRestartServer` would be necessary in this case).<br>
2) Edit the template files at `~/.vim/templates/` to customize the templates for `.h`, `.hpp`, `.c` and `.cpp`.<br>
3) `<C-]>` is set as `2<C-]>` in `.vim` files, because the second option is normally what you need. If not, type `g<C-]>` then a number to do your choice.<br>
4) For huge/distributed projects, use `<F12>` before `<C-]>` is recommended if possible.<br>
5) If `tags file not ready` is printed while typing `<C-]>` or `g<C-]>`, it means that the file `.tags` hasn't been generated yet by `ctags`. It is probably because that the project is so huge that `ctags` needs some time to generate the `.tags`.<br>
6) After typing `C-p` and selecting a file, type `F12` is recommanded to refresh the Nerdtree.<br>
7) `<C-l>` may be necessary to refresh the whole vim interface after some operations, such as `<F6>`.<br>
8) `vim -Nu ~/.vim/bundle/YouCompleteMe/vimrc_ycm_minimal xxx.cpp` to get minimal vim configuration for YCM to debug YCM.<br>
9) Debug commands of YCM: `:YcmDiags`, `YcmDebugInfo`, `YcmToggleLogs`.


### About syntax highlight ###
Custom names aren't recommended to use the used words in C++ Standard Library and in STL. So words like `count` from `int count;` would be highlighted as it is the function name coming from STL. If you want to get a custom name like `count` without highlight, you need to replace the line `systax keyword cppSTLfunction count` into `syntax match cppSTLfunction "\(\.|-\>\)\@<=count"` in the file `cpp.vim` in `~/.vim/after/syntax/`.<br>
The syntax highlight works based on the regular experssion. So long codes in one line may cause delay. To avoid this, the syntax highlight works on the lines with the length smaller than 600, the part of over 600 won't be highlighted.



### References ###
https://github.com/VundleVim/Vundle.vim<br>
https://github.com/Valloric/YouCompleteMe<br>
https://github.com/rdnetto/YCM-Generator<br>
https://github.com/preservim/nerdtree<br>
https://github.com/ctrlpvim/ctrlp.vim<br>
https://github.com/scrooloose/nerdcommenter<br>
https://github.com/vim-scripts/taglist.vim<br>
https://github.com/vim-airline/vim-airline<br>
https://github.com/vim-airline/vim-airline-themes<br>
https://github.com/rstacruz/sparkup<br>
https://github.com/Yggdroot/indentLine<br>
https://github.com/jiangmiao/auto-pairs<br>
https://github.com/tpope/vim-repeat<br>
https://github.com/tpope/vim-fugitive<br>
https://github.com/Xuyuanp/nerdtree-git-plugin<br>
https://github.com/YvesZHI/vim-code-dark<br>
https://github.com/YvesZHI/vim-cpp-enhanced-highlight<br>
https://github.com/YvesZHI/killersheep<br>
https://codeyarns.com/2015/03/18/how-to-test-color-setup-in-vim
