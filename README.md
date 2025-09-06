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

Install the plugin manager `tmp` by running the following command

```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

## Step 3 

Copy the following content into your new created .tmux.conf

## Warning ⚠: Make sure you've already installed `xclip` package in your system otherwise run `sudo apt install xclip`

### X-11 SESSION
```bash
# set -g default-shell /bin/bash

# Set the default terminal to a 256-color screen
set -g default-terminal "screen-256color"

# Set the prefix key to Ctrl-w
set -g prefix C-w
unbind C-b
bind-key C-w send-prefix

# Split panes vertically and horizontally
unbind %
bind | split-window -h

unbind '"'
bind - split-window -v

# Reload the configuration file
unbind r
bind r source-file ~/.tmux.conf

# Keybindings for resizing panes
# Resizing panes down, up, right, and left
bind -r j resize-pane -D 5
bind -r k resize-pane -U 5
bind -r l resize-pane -R 5
bind -r h resize-pane -L 5

# Ensure tmux recognizes the control key for pane switching
bind-key C-h if-shell "[[ $(tmux display -p '#{pane_in_mode}') -eq 1 ]]" "send-keys C-h" "select-pane -L"
bind-key C-j if-shell "[[ $(tmux display -p '#{pane_in_mode}') -eq 1 ]]" "send-keys C-j" "select-pane -D"
bind-key C-k if-shell "[[ $(tmux display -p '#{pane_in_mode}') -eq 1 ]]" "send-keys C-k" "select-pane -U"
bind-key C-l if-shell "[[ $(tmux display -p '#{pane_in_mode}') -eq 1 ]]" "send-keys C-l" "select-pane -R"

# Zoom/unzoom the current pane
bind -r m resize-pane -Z

# Enable mouse support for switching panes, scrolling, and resizing
set -g mouse on

# Use Vim keybindings in copy mode
set-window-option -g mode-keys vi

# Start selection in copy mode with 'v'
bind-key -T copy-mode-vi 'v' send -X begin-selection

# The tmux-yank plugin now handles copying to the system clipboard with 'y'.
# The following line is no longer necessary, as the plugin provides this functionality.
# bind-key -T copy-mode-vi 'y' send -X copy-selection

# Prevent exiting copy mode after dragging with the mouse
unbind -T copy-mode-vi MouseDragEnd1Pane

# Disable the bottom status line
set-option -g status off

# --- TPM (Tmux Plugin Manager) ---
set -g @plugin 'tmux-plugins/tpm'

# List of tmux plugins
set -g @plugin 'christoomey/vim-tmux-navigator' # For navigating panes and vim/nvim with Ctrl-hjkl
set -g @plugin 'jimeh/tmux-themepack'          # For configuring a tmux theme
# set -g @plugin 'tmux-plugins/tmux-resurrect'    # To persist tmux sessions after a computer restart
set -g @plugin 'tmux-plugins/tmux-continuum'   # Automatically saves sessions for you every 15 minutes
set -g @plugin 'tmux-plugins/tmux-yank'         # The new plugin to copy to the system clipboard

# Tmux-yank configuration
# Use xclip on Linux (common)
set -g @tmux-yank 'xclip'
# Uncomment the line below for xsel on Linux
# set -g @tmux-yank 'xsel'
# Uncomment the line below for pbcopy on macOS
# set -g @tmux-yank 'pbcopy'

# Theme configuration
set -g @themepack 'powerline/default/cyan'

# Tmux-resurrect and tmux-continuum settings
# set -g @resurrect-capture-pane-contents 'on' # Allow tmux-ressurrect to capture pane contents
# set -g @continuum-restore 'on'               # Enable tmux-continuum functionality

# --- Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf) ---
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



## Step 4

Start the tmux by running the following command

```bash
tmux
```

Then press  `ctrl+w` then `shift+I`. It will install all the plugins you needed. Once installation is complete then restart your tmux session.

# You are done
