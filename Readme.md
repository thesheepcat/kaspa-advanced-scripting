## RedeemScript:

b.AddData(sig)

b.AddData(secret)

b.AddData(contract)

## plainredeemScript: 
 
b.AddData(sig)
533adebc09aeaa90eafb3c0ffda791b245dd7fc1b5a267e62e3532e73f80e79465cec397824e0841fff49e2e9c7f122a65464263dc45be9ad5f8897828d44da101

b.AddData(secret)
c66531fb402f0088d9f5be954cbfededef83a9d3100ef028d57c5aef2dedba3a

b.AddData(contract) 
a820d87584e4523ea6597caa8fa9bf263a0456f525e59955fc7224a832c51053845e8820da548c2998c850ebdb39e0bc95d9461bf9f6c4640671655fa4112be194f8a226ac


## PlainSecretContract to redeem: 

OP_SHA256 
d87584e4523ea6597caa8fa9bf263a0456f525e59955fc7224a832c51053845e 
OP_EQUALVERIFY 
da548c2998c850ebdb39e0bc95d9461bf9f6c4640671655fa4112be194f8a226 
OP_CHECKSIG


## Operations sequence on the stack:
1. At first, script engine add on the stack all data from Redeem Script;
2. It adds the signature on the stack (stepping 00:0000: OP_DATA_65)
3. It adds the secret on the stack (stepping 00:0001: OP_DATA_32)
4. It adds the contract on the stack (stepping 00:0002: OP_DATA_69)
5. After adding all data from RedeemScript, script engine execute OP_BLAKE2B on the top element (the contract) and adds back the contract hash on the stack (stepping 01:0000: OP_BLAKE2B)
6. The script engine adds an additional data on the stack (i expect it to be the hashed contract that's present in the locking script P2SH of the UTXO i'm trying to spend) (stepping 01:0001: OP_DATA_32); at that point, on the stack i can see the top 2 elements are equal;
7. The script engine execute OP_EQUAL to check if the two top elements of the stack are equal (stepping 01:0002: OP_EQUAL); they are equal and the script engine removes them from the stack;
8. The script engine elaborate the HASH SHA256 of the first item in the stack (the secret) and add the calculated hash back to the stack (stepping 02:0000: OP_SHA256);
9. The script engine adds to the stack the secret hash include in the contract (stepping 02:0001: OP_DATA_32); at that point, on the stack i can see the top 2 elements are equal;
10. The script enegine verifies the two top elements of the stack (the hashed secrets) and remove them from the stack (stepping 02:0002: OP_EQUALVERIFY);
11. The script engine adds the pubkey from the contract in the stack (stepping 02:0003: OP_DATA_32);
12. The script engine verifies that the signature is correctly verifying the pubkey (stepping 02:0004: OP_CHECKSIG);
13. The script has been successfully verified and the UTXO can be spent.


