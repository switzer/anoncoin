Zerocoin
=====================

None of the following scripts are actually executed; instead, the ZC opcodes
are recognized and so that input or output is ignored by the regular Anoncoin
transaction processing (this is why they can be prefix rather than postfix).
This transaction is marked for Zerocoin processing. All values are VarInt
unless otherwise specified.

New input type:

* ZC spend: can have a maximum of one in a transaction, to reduce
  implementation complexity.
  Amount must be one of the denomination amounts.
  Script format: `ZCSPEND version serialNum spendRootHash`.

New output types:

* ZC checkpoint: always has an amount of zero.
  Script format: `RETURN ZCCHECKPT version denom accum1value .. accumNvalue`.
  These can only occur in the coinbase transaction. A checkpoint output can
  occur at most once for any given denomination, and is required if any ZC mints
  using that denomination occur in the block.

* ZC mint: amount must be one of the denomination amounts.
  Script format: `RETURN ZCMINT version publicCoinValue`.


???WHERE DO I PUT THIS??? `spendTxHash` is produced by taking the binary
  representation of the ZC spend transaction as found in "tx" network messages,
  and replacing the input's spendRootHash with all zero bytes.



### Format of Zerocoin spend roots (hashes are denoted spendRootHash, above)

Contents:

* the block hash containing the accumulator checkpoint

* N hashes of accPoKs (one for each UFO)

* 4 hashes of snSoK s, s': each part corresponds to a separate 20 bits of the
  challenge hash, and thus are independently verifiable.

* 4 snSoK t hashes; each part corresponds to a separate 20 bits of the
  challenge hash.

* `commitmentPoK`

* `commitmentToCoinUnderSerialParams`

* `commitmentToCoinUnderAccParams`