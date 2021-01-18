# Simple benchmark of hash algorithms

Code and results when ran on Windows 10 64bits, i7 5820K, PHP 8.0.0:

```php
<?php // Run this in your terminal with: php hash_speed_simple_benchmark.php

$loops = 100000;
$str = "This string will be hashed";
$results = [];
$hash = '';
$largestAlgoNameLength = 1;
foreach (hash_algos() as $algo) {
	if (strlen($algo) > $largestAlgoNameLength) {
		$largestAlgoNameLength = strlen($algo);
	}
	$tss = microtime(true);
	for ($i = 0; $i < $loops; $i++) {
		$hash = hash($algo, $str, false);
	}
	$tse = microtime(true);
	$results[$algo] = sprintf("%'06.4f", $tse - $tss) . "\t$hash";
}
asort($results);
foreach ($results as $algo => $result) {
	echo str_pad($algo, $largestAlgoNameLength) . "  $result\n";
}

/* Output I got on Windows 10 64 bits Intel i7 5820k:

fnv132       0.0418     384f2e7c
fnv1a32      0.0418     b41d18f7
joaat        0.0426     f50d3966
fnv164       0.0429     bbc68788dd9982fd
fnv1a64      0.0430     2e5e06a6beec59f7
crc32b       0.0450     79055994
crc32        0.0451     594ede08
crc32c       0.0454     c8c4f3b3
adler32      0.0466     07e601fe
md4          0.0571     9e0b1ccdad62e7933fb23b4e548f7aeb
tiger128,3   0.0574     4d69f33416c184ed0334865558c10f60
tiger160,3   0.0582     3e51e31673b428f015ec0b3244aabd224b2d72e8        
md5          0.0584     bf24c1554b06c7807decff4292aaa357
tiger192,3   0.0593     1c8a180b5d35f4de523f4e6feb6ae62e80f050a20ce1f06a
tiger128,4   0.0608     1b7dca09fd73a259c4a2514f92f20392
tiger192,4   0.0620     b90b8298e54ebef79af401dbaedebf11bd87dd22d31725a6
tiger160,4   0.0621     50b533fc70520e963e02bf14525fcb8750078f6d        
sha1         0.0626     687c4c3c671f41c270aad5fdc3035c66d7f8b4e9        
ripemd128    0.0784     e897f3f2e514c4d3a01ce77fbfe0e705
ripemd160    0.0880     36f4193a4cd6433262bb826e110f45b8548830ed        
haval160,3   0.0958     ad3267a94ef417b447d1b00a64330c28aa6da582        
haval128,3   0.0967     74551cdf49d24dfb7789a8f2a6440b64
sha512/224   0.0967     a8bb461c68981e813efe223fbb6dd811834105e4cdf8ab82339b950c
sha512/256   0.0968     b61066f3d3d46a4c94c4f1983477e4999b1ca0d691fb9d42072b810e40e36df7
haval192,3   0.0972     f49a7248a7924d72e7438c4d62479fe35623dd10086c43d5
haval256,3   0.0973     4e40d21f3c7efae4886d8c531200c29ef1e67d49b02cf325eaa628e2e396b746
sha3-256     0.0977     fc8dd08bbc48abb9b3e95449f43edc6f1f6f50d7972a718183e8efc546bcde69
sha3-384     0.1000     0595959308a91b9304b3c1abe4e27dac456eca67cf569f6c6fb2f05229eb6670eb12a5713eb93d747c8ac157aa9c277f
haval224,3   0.1001     174ed3d1274c81c95e887aa8900ce0bc8d61748aa64bc28b1cccb28c
sha384       0.1001     4e56596fffeb4337c882d31c18ccc73a21e8d8306992b1077914cd0b7838d19cb6440d3a933ded83a9b4797e1b966541
sha3-224     0.1001     56e4d5835a6da27c507268a4bf9a2f9fb2104755d8c8f8a91d0331e3
ripemd256    0.1047     0eefedae3adf39ac6f30f36d166d0b1c5f9636e99102a8957e492fe31491c96a
haval256,4   0.1165     c38cb98c534812c07eb21f2fc4c9eb8f48240faa544f743325734b320300e6f1
sha224       0.1201     93df8fc779b529265d60131ef4c7eafcf91ebf92bdf69577087cc1a4
haval160,4   0.1255     7a161cdfd82446eaed25bf1c75a3092fb0cc798c
haval192,4   0.1259     2ef733ef86d83d5fba2c1671b0b672fca063a0508071186a
haval224,4   0.1266     892c9521b27ef34a1b9691dd1920d229fa140d772d2c0f608be38d75
sha256       0.1271     7c731dc407f00c09a5a32834dc7de110a6d9df1f54fc09ec2c6521a96685f6cf
ripemd320    0.1302     0645a494157f419259d180052f355e9f28cb63b5eab968b6f17ff0393967b26b8c97105e0bc0c5ca
haval224,5   0.1310     3e806cf7506924dab38f9e9fd9d819d50d6acca31ee3874c2d150e4c
haval128,5   0.1314     036733e6407916e4e1808b3bbf691c02
haval256,5   0.1316     d40bfd2063fb49146c092255d2c8d41e4ca7ffe787f7857ed717a9455dd35f74
haval160,5   0.1367     9127e6eeaa610f283b1ff6e3be3ceea3f1a9ef17
haval192,5   0.1390     a4578e72f8b56c8ce5f37666858ec38b2e1ad7e19f510d32
sha512       0.1409     871617e7cac4486c8a6b6b5712ef2e9b9ececbce20247599178234f7c1bf8181657ef6aac539b02e5dec604df1c20b674c8da8da07acad343a2464704b65bfc5
haval128,4   0.1475     5957d0d9350f3b08aa77776f4c91537f
sha3-512     0.1530     18912de10991651921cc12ee3c7b64d0c5ab1819269ac64410ce638af3086a43e6282c22fc350dd51570fbb2f28cb970106f9857285ccea1dd813ba878f7b0a6
whirlpool    0.2419     e48d5078f61305bc4e2cf15cf652c3513a304eb3d38240777fe6ee505f2e6ded4aefcd9723e121c4939cfbe3fb3ebe37679367f06032ca788c502577bd5d8a08
gost-crypto  0.2846     50f0f6e8e49c5d890e9978bc06c168878940ab97ef23de52fc442c4248877cf0
gost         0.2847     eda0caa196f9b18a3c5de3adffba40bb4cdf50808ef831530873bee2f18fdc43
snefru256    0.4264     e1fc6cf4f178d10ffea1a0d299702b84beb490016cb85ed731c5b63338d8a302
snefru       0.4273     10ba5b1b0a000841c2eac7cb9a6db249885314389751f92f397d228c22bd61da
md2          1.2724     89ed38d9c1970f89c2c262db648d00fb
*/
```
