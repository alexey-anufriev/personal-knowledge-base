# Basics

## How to calculate Password Entropy

$$
E = log_2(S^L)
$$

**Where**: S - set of characters, L - password length.

**Example**: password consists of character used by `base64` encoding has a length of 10 symbols.

$$
E = log_2(64^{10}) = 60 bits
$$

![](../.gitbook/assets/entropy.png)

