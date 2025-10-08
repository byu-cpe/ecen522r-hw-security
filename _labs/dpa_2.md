---
layout: lab
toc: true
title: "Side Channel: DPA (Part 2)"
short_title: "Side Channel: DPA 2"
number: 7
under_construction: false
---

In this lab you will use the round 1 key you obtained from your DPA attack last lab in order to obtain the full DES key. 

## Preliminary

The round 1 key is 48 bits in size; whereas the full DES key is 64-bits.  You will need to figure out how to obtain the full 64-bit key from the 48-bit round key.  Once you have the original 64-bit key, you can test it on the provided plaintext/ciphertext pairs in the dataset to verify you have the correct key.

Some important points to get you started:
* Although the DES key is 64 bits, only 56 bits are actually used by the DES algorithm.  The other 8 bits are parity bits.  If you use the `des.py` code provided in the *lab_dpa_ii/* directory to verify your key, the parity bits are ignored, so just set them to 0.
* The key schedule algorithm takes the 56-bit key and generates a 48-bit key for each round.  You have the 48-bit round 1 key obtained from your DPA attack; however, since you don't know the key in other rounds, you will need to figure out how to obtain these other 8 missing bits. 
* I suggest you start by reading through this page: <https://ritul-patidar.medium.com/key-expansion-function-and-key-schedule-of-des-data-encryption-standard-algorithm-1bfc7476157>.  It has a great description of the DES key schedule algorithm.  With some simple Python code, you can work backwards through the key schedule algorithm to go from the round 1 key back to the original key.

## Instructions
1. Write a Python program that does the following:
    * Has your round 1 key hardcoded in the beginning of the file.
    * Computes the 64-bit key using the round 1 key.
    * Verifies the key by checking several plaintext/ciphertext pairs in the dataset (these can be hardcoded in the file; you do not need to load the full dataset in your script).
1. Save your key in a text file, `lab_dpa_ii/key.txt`, as an ASCII hex string.  For example, if your key is `0x0123456789ABCDEF`, then the contents of *key.txt* should be:

    ```
    0123456789ABCDEF
    ```

    Again, the parity bits are ignored, so for simplicity and consistency, please set them to 0. 

## What to Submit

Tag your submission as `lab_dpa_ii` and make sure to commit:
1. Your Python program that computes the 64-bit key (place it in `lab_dpa_ii/`).
1. Your text file containing the 64-bit key (`lab_dpa_ii/key.txt`).

## Helpful Tips

### DES Data Structures
The *des.py* file provided to you has several data structures that may be helpful in this lab.  For example, the `pc1_table` is used to permute the 64-bit key to a 56-bit key.  By making use of these data structures, you can implement this lab with a small amount of Python code.

When I implemented this lab, I stored the key as a Python string (or list) of 1s and 0s.  This made it very easy to manipulate the key bits and make use of the data structures provided in *des.py*.