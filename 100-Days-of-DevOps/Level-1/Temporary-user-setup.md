# =========================================

# SSH ACCESS + USER EXPIRY CONFIGURATION

# =========================================

# 1. SSH into target server (correct command)

ssh banner@stapp03

# If first-time connection, accept host key

# type: yes

# 2. Switch to root user

sudo su -

# 3. Create new user

adduser mark

# 4. Set account expiry date

chage -E 2027-01-28 mark

# 5. Verify user expiry settings

chage -l mark

# =========================================

# EXPECTED OUTPUT (important fields)

# =========================================

# Account expires : Jan 28, 2027

# Password expires: never

# =========================================
