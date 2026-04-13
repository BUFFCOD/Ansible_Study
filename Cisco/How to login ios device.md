


run this command 


sudo update-crypto-policies --set LEGACY

after try to ssh again


What the LEGACY Policy Allows
Setting	DEFAULT	LEGACY
TLS 1.0	Disabled	Enabled
TLS 1.1	Disabled	Enabled
SHA-1	Disabled for most uses	Allowed
Minimum RSA key	2048 bits	1024 bits
Minimum DH parameter	2048 bits	1024 bits
3DES	Disabled	Enabled
CBC mode	Allowed	Allowed
DSA keys	Disabled	Enabled
RC4	Disabled	Limited use
Switching to the LEGACY Policy

Copy
# Apply the LEGACY policy

sudo update-crypto-policies --set LEGACY

# Verify the change
update-crypto-policies --show
# Output: LEGACY

# Restart affected services
sudo systemctl restart sshd