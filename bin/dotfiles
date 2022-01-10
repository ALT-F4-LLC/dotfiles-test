#!/bin/bash
set -e

DOTFILES_DIR="$HOME/.dotfiles"
SSH_DIR="$HOME/.ssh"

if ! [ -x "$(command -v ansible)" ]; then
  sudo pacman -S ansible --noconfirm
fi

if ! [[ -f "$SSH_DIR/id_rsa" ]]; then
  mkdir -p "$SSH_DIR"

  chmod 700 "$SSH_DIR"

  ssh-keygen -b 4096 -t rsa -f "$SSH_DIR/id_rsa" -N "" -C "$USER@$HOSTNAME"

  cat "$SSH_DIR/id_rsa.pub" >> "$SSH_DIR/authorized_keys"

  chmod 600 "$SSH_DIR/authorized_keys"
fi


if [[ -f "$DOTFILES_DIR/requirements.yml" ]]; then
  cd "$DOTFILES_DIR"

  ansible-galaxy install -r requirements.yml
fi

cd "$DOTFILES_DIR"

if [[ -f "$DOTFILES_DIR/vault-password.txt" ]]; then
  ansible-playbook --diff --vault-password-file "$DOTFILES_DIR/vault-password.txt" "$DOTFILES_DIR/main.yml"
else
  ansible-playbook --diff "$DOTFILES_DIR/main.yml"
fi
