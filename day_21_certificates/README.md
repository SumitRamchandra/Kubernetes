Create private key

In this step, you create a private key. You need to keep this document secret; anyone who has it can impersonate the user.

# Create a private key (Change the common name "myuser")
openssl genrsa -out myuser.key 3072

# Change the common name "myuser" to the actual username that you want to use
openssl req -new -key myuser.key -out myuser.csr -subj "/CN=myuser"
