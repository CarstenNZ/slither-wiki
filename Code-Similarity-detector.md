# Code Similarity Detector

`slither-simil` is a tool designed to use state-of-the-art Machine Learning to detect similar Solidity functions.
Our tool should be trained with a large corpus of contracts to work. We offer a pretrained model from [etherscan_verified_contracts](https://github.com/thec00n/etherscan_verified_contracts) with 60,000 contracts counting more than 850,000 functions.
`slither-simil` uses a vector embedding technique called [FastText](https://github.com/facebookresearch/fastText) to generate a compact numerical representation of every function. We use this library because:
* it implements several state-of-the-art techniques such as nbow and skipgrams,
* it has high performance (it is C++ code with Python bindings),
* it has MIT license,
* it is well maintained (by Facebook).
 
## Requierements

Before start using `slither-simil`, install the required packages:

```
$ pip3 install pybind11 --user
$ pip3 install https://github.com/facebookresearch/fastText/archive/0.2.0.zip --user
$ pip3 install matplotlib --user
```

Make sure that you are using pip3.6 or later. If you are running from inside a [virtualenv](https://virtualenv.pypa.io/en/latest/), remove the `--user` parameter.

## Usage

All these examples will use the following files:

* [etherscan_verified_contracts.bin](https://drive.google.com/file/d/1oEhbIL4V9582Y5VKp4iiOURGq8qa4cBN/view?usp=sharing)
* [cache.npz](https://drive.google.com/file/d/1vpwusbyzLn1JqqAvlFivHXtLvsEp0VqX/view?usp=sharing)
* [MetaCoin.sol](link)

Our tool has several modes:
- `info`: prints the raw representation of a function and its information about a function

### Info mode

This mode has two features. You can either inspect the internal information about a pre-trained model:

```
$ slither-simil info etherscan_verified_contracts.bin 
etherscan_verified_contracts.bin uses the following words:
</s>
index(uint256)
return
condition(temporary_variable)
member
solidity_call(require(bool))
library_call
binary(+)
event
(local_solc_variable(default)):=(temporary_variable)
binary(==)
...
```

or examine the internal representation of function:

```
$ slither-simil info etherscan_verified_contracts.bin --filename MetaCoin.sol --contract MetaCoin --fname sendCoin
Function sendCoin in contract MetaCoin is encoded as:
index(uint256) binary(<) condition(temporary_variable) return index(uint256) binary(-) index(uint256) binary(+) event return
[ 0.00689753 -0.05349572 -0.06854086 -0.01667773  0.1259813  -0.05974023
  0.06719872 -0.04520541  0.13745852  0.14690697 -0.03721125  0.00579037
...
```

### Test mode

Test mode will requieres a pre-trained model, a filename, a contract name, a function name and an input directory or file. The input can be either a directory with contracts or special cache file with a pre-computed list of vectors for every contract. It is very recommended to use the cache to avoid longer processing times compiling and vectorizing the input contracts.  

```
$ slither-simil test etherscan_verified_contracts.bin --filename MetaCoin.sol --contract MetaCoin --fname sendCoin --input cache.npz --ntop 25
INFO:Slither-simil:Reviewed 825062 functions, listing the 25 most similar ones:
('0x954b5de09a55e59755acbda29e1eb74a45d30175_Fluz.sol', 'Fluz', 'transfer') 1.0
('0x55648de19836338549130b1af587f16bea46f66b_Pebbles.sol', 'Pebbles', 'transfer') 1.0
('0x3fcee23add6e86dde3c4d395cbce1cae7f16d06d_SnipCoin.sol', 'SnipCoin', 'sendCoin') 1.0
('0x000000005fbe2cc9b1b684ec445caf176042348e_ProperProposal.sol', 'Vote', 'transfer') 1.0
('0x000000002bb43c83ece652d161ad0fa862129a2c_AccountRegistry.sol', 'Vote', 'transfer') 1.0
('0x4e84e9e5fb0a972628cf4568c403167ef1d40431_Fluzcoin.sol', 'Fluzcoin', 'transfer') 1.0
('0x334eec1482109bd802d9e72a447848de3bcc1063_AirDropToken.sol', 'AirDropToken', 'transfer') 1.0
('0x28ccdda197d319a241005b9c9f01bac48b90f556_AirDropToken.sol', 'AirDropToken', 'transfer') 1.0
('0x000000002647e16d9bab9e46604d75591d289277_Vote.sol', 'Vote', 'transfer') 1.0
('0xc6c4c7826D44ABF22c711E8E86bDC3f5242d2182_token.sol', 'token', 'sendCoin') 1.0
('0x22033df1d104736ff4c2b23a28affe52863ca9c8_AtmOnlyFluzcoin.sol', 'AtmOnlyFluzcoin', 'transfer') 1.0
('0xcad796d6a2c0bb1de7f24262819be96fb08c1c3a_Love.sol', 'Love', 'transfer') 1.0
('0x6cb2b8dc6a508c9a21db9683d1a729715969a6ee_TokenEscrow.sol', 'TokenEscrow', 'transferFromOwner') 0.99582136
('0xd75fefe3cdb647281eec3f8fc738e3bc9658f9e4_ProofOfReadToken.sol', 'ProofOfReadToken', 'transfer') 0.99582136
('0x7A8Ef7E8c8f16B9D6F39069ce03d752Af23b46d6_OBS_V1.sol', 'MyObs', 'transfer') 0.99582136
('0x5ac0197c944c961f58bb02f3d0df58a74fdc15b6_TokenEscrow.sol', 'TokenEscrow', 'transferFromOwner') 0.99582136
('0x69719c8c207036bdfc3632ccc24b290fb7240f4a_BitPayToken.sol', 'BitPayToken', 'transfer') 0.99582136
('0x3d8a10ce3228cb428cb56baa058d4432464ea25d_TestToken.sol', 'TestToken', 'transfer') 0.99254405
('0x69719c8c207036bdfc3632ccc24b290fb7240f4a_BitPayToken.sol', 'BitPayToken', 'transferFrom') 0.991822
('0x486e1f44b2a85150a6dd2de5aab87df375cd8880_CAIRToken.sol', 'StandardToken', 'transfer') 0.9910706
('0x2bec16b164725efc192b7ec0296f838c61317514_eda.sol', 'StandardToken', 'transfer') 0.9910706
('0xb7cb1c96db6b22b0d3d9536e0108d062bd488f74_WaltonToken.sol', 'StandardToken', 'transfer') 0.9910706
('0x346c3be6aebEBaF5Cb766a75aDc9827EfbB7E41A_DelphiToken.sol', 'StandardToken', 'transfer') 0.9910706
('0xf5068761511594c82328102f4fde4650ed9ea6c4_WHP.sol', 'WHP', 'transfer') 0.99052656
('0x5f9f2ae7150d0beef3bb50ac8d8f4b43e6a6cc57_NABC.sol', 'NABC', 'transfer') 0.99052656
```

Search similar functions in more than 800,000 functions only takes 20 seconds.


### Train mode

...

### Plot mode

...