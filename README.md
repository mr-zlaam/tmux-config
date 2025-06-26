## Step 1 

If you haven't installed the tmux then install it by running the following command
### MAC
```bash
brew install tmux
```
### LINUX
```bash
sudo apt install tmux 	# for debian base distros
```

**Next**

Create a file called `.tmux.conf` in your home directory using the `touch` command.
```bash
touch ~/.tmux.conf
```
## Step 2 

Copy the following content into your new created .tmux.conf

### X-11 SESSION
```bash
set -g default-terminal "screen-256color"

set -g prefix C-w
unbind C-b
bind-key C-w send-prefix

unbind %
bind | split-window -h

unbind '"'
bind - split-window -v

unbind r
bind r source-file ~/.tmux.conf
# for moving between pans
bind -r j resize-pane -D 5
bind -r k resize-pane -U 5
bind -r l resize-pane -R 5
bind -r h resize-pane -L 5

# Ensure tmux recognizes the control key for pane switching
bind-key C-h if-shell "[[ $(tmux display -p '#{pane_in_mode}') -eq 1 ]]" "send-keys C-h" "select-pane -L"
bind-key C-j if-shell "[[ $(tmux display -p '#{pane_in_mode}') -eq 1 ]]" "send-keys C-j" "select-pane -D"
bind-key C-k if-shell "[[ $(tmux display -p '#{pane_in_mode}') -eq 1 ]]" "send-keys C-k" "select-pane -U"
bind-key C-l if-shell "[[ $(tmux display -p '#{pane_in_mode}') -eq 1 ]]" "send-keys C-l" "select-pane -R"

# For resizing pan
bind -r m resize-pane -Z

set -g mouse on

set-window-option -g mode-keys vi

bind-key -T copy-mode-vi 'v' send -X begin-selection # start selecting text with "v"
bind-key -T copy-mode-vi 'y' send -X copy-selection # copy text with "y"

unbind -T copy-mode-vi MouseDragEnd1Pane # don't exit copy mode after dragging with mouse
# disable bottom line
set-option -g status off
# tpm plugin
set -g @plugin 'tmux-plugins/tpm'

# list of tmux plugins
set -g @plugin 'christoomey/vim-tmux-navigator' # for navigating panes and vim/nvim with Ctrl-hjkl
set -g @plugin 'jimeh/tmux-themepack' # to configure tmux theme
#set -g @plugin 'tmux-plugins/tmux-resurrect' # persist tmux sessions after computer restart
set -g @plugin 'tmux-plugins/tmux-continuum' # automatically saves sessions for you every 15 minutes

set -g @themepack 'powerline/default/cyan' # use this theme for tmux

set -g @resurrect-capture-pane-contents 'on' # allow tmux-ressurect to capture pane contents
set -g @continuum-restore 'on' # enable tmux-continuum functionality

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run '~/.tmux/plugins/tpm/tpm'
```
### wayland -session

```bash
# Make sure you've installed "wl-copy" in your system
set -g default-terminal "screen-256color"

set -g prefix C-w
unbind C-b
bind-key C-w send-prefix

unbind %
bind | split-window -h

unbind '"'
bind - split-window -v

unbind r
bind r source-file ~/.tmux.conf
# for moving between pans
bind -r j resize-pane -D 5
bind -r k resize-pane -U 5
bind -r l resize-pane -R 5
bind -r h resize-pane -L 5

# Ensure tmux recognizes the control key for pane switching
bind-key C-h if-shell "[[ $(tmux display -p '#{pane_in_mode}') -eq 1 ]]" "send-keys C-h" "select-pane -L"
bind-key C-j if-shell "[[ $(tmux display -p '#{pane_in_mode}') -eq 1 ]]" "send-keys C-j" "select-pane -D"
bind-key C-k if-shell "[[ $(tmux display -p '#{pane_in_mode}') -eq 1 ]]" "send-keys C-k" "select-pane -U"
bind-key C-l if-shell "[[ $(tmux display -p '#{pane_in_mode}') -eq 1 ]]" "send-keys C-l" "select-pane -R"

# For resizing pan
bind -r m resize-pane -Z

set -g mouse on


set-window-option -g mode-keys vi
# Clipboard integration
set -g set-clipboard on
# OLD: set -s copy-command 'xclip -in -selection clipboard' # <-- COMMENT OUT OR REMOVE THIS
set -s copy-command 'wl-copy' # <-- USE WL-COPY FOR WAYLAND

# OLD: bind-key -T copy-mode-vi 'y' send-keys -X copy-pipe-and-cancel "xclip -in -selection clipboard" # <-- COMMENT OUT OR REMOVE THIS
bind-key -T copy-mode-vi 'y' send-keys -X copy-pipe-and-cancel "wl-copy" # <-- USE WL-COPY FOR WAYLAND

# OLD: bind-key -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "xclip -in -selection clipboard" # <-- COMMENT OUT OR REMOVE THIS
bind-key -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "wl-copy" # <-- USE WL-COPY FOR WAYLAND

unbind -T copy-mode-vi MouseDragEnd1Pane # don't exit copy mode after dragging with mouse
# disable bottom line
set-option -g status off
# tpm plugin
set -g @plugin 'tmux-plugins/tpm'

# list of tmux plugins
set -g @plugin 'christoomey/vim-tmux-navigator' # for navigating panes and vim/nvim with Ctrl-hjkl
set -g @plugin 'jimeh/tmux-themepack' # to configure tmux theme
#set -g @plugin 'tmux-plugins/tmux-resurrect' # persist tmux sessions after computer restart
set -g @plugin 'tmux-plugins/tmux-continuum' # automatically saves sessions for you every 15 minutes

set -g @themepack 'powerline/default/cyan' # use this theme for tmux

set -g @resurrect-capture-pane-contents 'on' # allow tmux-ressurect to capture pane contents
set -g @continuum-restore 'on' # enable tmux-continuum functionality

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run '~/.tmux/plugins/tpm/tpm'

```

## Step 3

Install the plugin manager `tmp` by running the following command

```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

## Step 4

Start the tmux by running the following command

```bash
tmux
```

Then press  `ctrl+shift+I`. It will install all the plugins you needed. Once installation is complete then restart your tmux session.

# You are done
