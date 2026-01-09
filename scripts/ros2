#!/bin/sh
set -e

# Detect OS information
if [ -f /etc/os-release ]; then
	. /etc/os-release
else
	echo "Error: /etc/os-release not found. Is this an Ubuntu system?" >&2
	exit 1
fi

# Verify it is Ubuntu
if [ "${ID}" != "ubuntu" ]; then
	echo "Error: This script is designed only for Ubuntu. Detected: ${ID}" >&2
	exit 1
fi

# Map Ubuntu codename to ROS 2 LTS distribution
ROS_DISTRO=""
case "${VERSION_CODENAME}" in
jammy)
	ROS_DISTRO="humble"
	;;
noble)
	ROS_DISTRO="jazzy"
	;;
focal)
	ROS_DISTRO="foxy"
	;;
*)
	echo "Error: Unsupported Ubuntu version (${VERSION_CODENAME})." >&2
	exit 1
	;;
esac

echo "Detected Ubuntu ${VERSION_CODENAME} (${VERSION_ID}). Target ROS 2 Distro: ${ROS_DISTRO}"

echo "Setting up locales..."
sudo apt-get update >/dev/null
sudo apt-get install -y locales >/dev/null
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

echo "Setting up sources..."
sudo apt-get install -y software-properties-common >/dev/null
sudo add-apt-repository -y universe >/dev/null

sudo apt-get update >/dev/null
sudo apt-get install -y curl gnupg lsb-release >/dev/null

echo "Adding ROS 2 GPG key..."
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg

echo "Adding ROS 2 repository..."
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu ${VERSION_CODENAME} main" | sudo tee /etc/apt/sources.list.d/ros2.list >/dev/null

echo "Installing ros-${ROS_DISTRO}-desktop..."
sudo apt-get update >/dev/null
sudo apt-get install -y "ros-${ROS_DISTRO}-ros-base"

echo "Installing development tools (ros-dev-tools)..."
sudo apt-get install -y ros-dev-tools >/dev/null

echo "===================================================="
echo "Installation Complete!"
echo "===================================================="
