# Mind-your-Ps-and-Qs
PicoCTF picoGym Practice Challenges | Mind your Ps and Qs

In this challenge we will get to know RSA algorithm. In plain English, RSA algorithm is used to send “encrypted” text, AKA cyphertext.

But how does this RSA algo works? We can always Google it to get into more technical terms but in plain text,
Imagine you have a lock which has 2 keys. You use one of them to lock (encrypt) it, and the other to open (decrypt) it.

So, we know for sure that it is of course the flag which has been encrypted. So in RSA algo the cyphertext is expressed as “c”.

If we download the “values” file and cat into it, we could see there are some values, c, n, e.

c = 861270243527190895777142537838333832920579264010533029282104230006461420086153423

n = 1311097532562595991877980619849724606784164430105441327897358800116889057763413423

'''e = 65537'''

We know what c is, probably the flag which has been somewhat encrypted by this RSA algo. But what are those “n” and “e”?

“e” is the one key which will lock it, AKA encrypt it.
“n” is a nothing but a big random number which is a product of 2 large prime numbers. The bigger the number of “n” the harder it is to break the lock, AKA harder it is to decrypt it. But how hard are we talking about? For that we need to know those simple formulas of encrypt and decrypt.

the encrypted text, c = (P to the power e) mod n.
the derypted text (or plain text), p = (C to the power d) mod n; here d is the other key which is used to decrypt the text.

The formula is, e.d = 1 mod ϕ(n).
Oh man now what is ϕ(n)? ϕ(n) is called totient function.
ϕ(n) = (p-1)*(q-1). Now what the hell these p and q are? Remember I told you n is product of 2 large prime numbers? These are the 2 prime numbers! But how do we get these 2 prime numbers? With any websites where we can calculate factors of any numbers. Here is a website where I did the factor calculation for this challenge.

![Screenshot (28)](https://user-images.githubusercontent.com/111799231/186301880-388579eb-8e70-4037-97c9-27d78746b89d.png)
You can see for these “n” which is 82 digits long, it took me 17 minutes 4 seconds. The longer the digits the longer it takes time to calculate the factor.

So, from the calculation of factors we have,
p = 1955175890537890492055221842734816092141
q = 670577792467509699665091201633524389157003

Now we know that, in RSA algo we have 3 main steps, 1. Generating the keys, 2. Encryption, 3. Decryption.

Enough talking, let’s start coding. Type python in webshell and code to get the flag,

c = 861270243527190895777142537838333832920579264010533029282104230006461420086153423

n = 1311097532562595991877980619849724606784164430105441327897358800116889057763413423

e = 65537

t = (p-1)*(q-1)
d = pow(e, -1, t)
print(d)
output: 693529123416505412979446025120625035374876994645029007711823240743237277989774953


plain_text = pow(c, d, n)
print(plain_text)
output: 13016382529449106065927291425342535437996222135352905256639573959002849415739773

print(f”Now converting this decimal value to hex value {hex(plain_text)}”)
output: Now converting this decimal value to hex value 0x7069636f4354467b736d6131315f4e5f6e305f67306f645f31333638363637397d

plain_text_ascii = bytearray.fromhex(hex(plain_text)[2:]).decode(‘ascii’)

print(plain_text_ascii)
output: picoCTF{sma11_N_n0_g0od_13686679}
