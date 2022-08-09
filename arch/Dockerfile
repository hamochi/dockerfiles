FROM archlinux

# Update and install some packages
RUN pacman -Syu
RUN pacman -Sy --noconfirm base-devel wget git neovim stow fish starship exa fd ripgrep fzf npm python-pip nodejs cargo go rust nerd-fonts

# Create a new user
ARG user=skywalker
RUN useradd --system --create-home $user && echo "$user ALL=(ALL:ALL) NOPASSWD:ALL" > /etc/sudoers.d/$user
USER $user
WORKDIR /home/$user

# Install yay and aur packages
RUN git clone https://aur.archlinux.org/yay.git \
  && cd yay \ 
  && makepkg -sri --needed --noconfirm 
RUN yay --save --nocleanmenu --nodiffmenu
RUN yes | yay -S dotacat bat
# RUN yes | yay -S bat

# Install Lunar Vim
ENV PATH="${PATH}:/home/$user/.local/bin"
RUN bash <(curl -s https://raw.githubusercontent.com/lunarvim/lunarvim/master/utils/installer/install.sh) --no-install-dependencies 
RUN rm -rf ~/.config/lvim

# Get my dotfiles 
RUN git clone https://github.com/hamochi/dotfiles.git \
  && cd dotfiles \ 
  && stow config

COPY starship.toml /home/$user/.config/starship.toml

