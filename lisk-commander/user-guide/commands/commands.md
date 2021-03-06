# Lisk Commander Commands

## List of commands

- show warranty
- show copyright
- [config](#configuration)
- [create account](#create-account)
- [show account](#show-account)
- [create transaction transfer](#create-transaction-transfer)
- [create transaction register second passphrase](#create-transaction-register-second-passphrase)
- [create transaction cast votes](#create-transaction-cast-votes)
- [create transaction create multisignature account](#create-transaction-create-multisignature-account)
- [create transaction register delegate](#create-transaction-register-delegate)
- [broadcast transaction](#broadcast-transaction)
- [broadcast signature](#broadcast-signature)
- [decrypt message](#decrypt-message)
- [decrypt passphrase](#decrypt-passphrase)
- [encrypt message](#encrypt-message)
- [encrypt passphrase](#encrypt-passphrase)
- [verify transaction](#verify-transaction)
- [sign transaction](#sign-transaction)
- [verify message](#verify-message)
- [sign message](#sign-message)
- exit
- [get](#get)
- [help](#help)
- [list](#list)
- [set](#set)


## Configuration

This command displays the current configuration.

**Input**
```shell
lisk> config
```

**Example JSON Output**
```json
 {
	"name": "Lisk Commander",
	"version": "1.0.0",
	"delimiter": "lisk",
	"json": false,
	"api": {
		"nodes": [],
		"network": "main"
	},
	"pretty": false
}
```

## Create Account

This command generates a random seed, along with the corresponding public key and address. It is the recommended way to create new accounts.

**Input**
```shell
lisk> create account
```

**JSON**
```json
{
	"passphrase": "account reform outdoor curtain animal zoo best gain super glue bacon endless",
	"privateKey": "c0554188319a911aec70a6e044cbf69ec0da19269d11e8cd4e2b5ee18afe4402f7425ba1b192e07639a0304531e21117ccc1852279b6ec7c296b18bd95bcc4c3",
	"publicKey": "f7425ba1b192e07639a0304531e21117ccc1852279b6ec7c296b18bd95bcc4c3",
	"address": "9292797545729948557L"
}
```

## Show Account

This command display the public key, private key and address which corresponds to the entered passphrase.

**Input**
```shell
lisk> show account
```

**JSON**
```json
{
        "privateKey": "a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3a4465fd76c16fcc458448076372abf1912cc5b150663a64dffefe550f96feadd",
        "publicKey": "a4465fd76c16fcc458448076372abf1912cc5b150663a64dffefe550f96feadd",
        "address": "12475940823804898745L"
}
```

## Create transaction transfer

This command creates and signs a type 0 transaction, which will transfer a Lisk balance to a provided address if broadcast to the network. The command takes two required parameters:

- An amount to transfer (in LSK).
- The address of the recipient.

**Example Input**
```shell
lisk> create transaction transfer 100 13356260975429434553L
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
```

**Example JSON Output**
```json
{
	"amount": "10000000000",
	"recipientId": "13356260975429434553L",
	"senderPublicKey": "caf0f4c00cf9240771975e42b6672c88a832f98f01825dda6e001e2aab0bc0cc",
	"timestamp": 64769338,
	"type": 0,
	"fee": "10000000",
	"recipientPublicKey": null,
	"asset": {},
	"signature": "097bbb6a740a2b90f44b903c0370a6c7ccca86eda6447998e85c745e77f82c2efaf80d9396de5c7a5d7be39a3e9029402b081f8c6f45dde67066d7668b75de05",
	"id": "17042051520078129298"
}
```

## Create transaction register second passphrase

This command creates and signs a type 1 transaction, which will register a second passphrase for the account if broadcast to the network.

**Example Input**
```shell
lisk> create transaction register second passphrase
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
Please enter your second secret passphrase: *******
Please re-enter your second secret passphrase: *******
```

**Example JSON Output**
```json
{
	"type": 1,
	"amount": 0,
	"fee": 500000000,
	"recipientId": null,
	"senderPublicKey": "6e0f31cd09bd602bf71960e4da1930ccd39b817d0a73986a09c344204ee1ec6b",
	"timestamp": 48028699,
	"asset": {
		"signature": {
			"publicKey": "a8ef35a53220246cce763ec98dbcf335b30b72d980e3e5cfe1cfcabd68581358"
		}
	},
	"signature": "5ad889263397837b52c7bedaa3bb0c906494a35ef940a410493cd5df1d654b0dbf6561d3a597f0463e5d88cdd8b9e87379266a1b351623cf9760875a2e575f0f",
	"id": "17851553801824463168"
}
```

## Create transaction register delegate

This command creates and signs a type 2 transaction, which will register the account as a delegate candidate if broadcast to the network. It has one required parameter which is the delegate username to be registered.

**Example Input**
```shell
lisk> create transaction register delegate username
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
```

**Example JSON Output**
```json
{
	"amount": "0",
	"recipientId": "",
	"senderPublicKey": "a4465fd76c16fcc458448076372abf1912cc5b150663a64dffefe550f96feadd",
	"timestamp": 64793730,
	"type": 2,
	"fee": "2500000000",
	"asset": {
		"delegate": {
			"username": "username"
		}
	},
	"signature": "4ef0dedacd5deba50785e115afca48d3db2427e8436e6fe8edb291ab420978cea75814ca58aac1a745da61c1cd5912103e3b8b8f2aed650622eb39d66b98bb01",
	"id": "12587307250270871466"
}
```

## Create transaction cast votes

This command creates and signs a type 3 transaction, which will cast votes or unvotes for delegates if broadcast to the network. The command requires at least one of the --votes and/or --unvotes options. These options can be specified either by a list of public key strings (corresponding to the delegates to be voted for/unvoted) separated by commas, or via a path to a file containing the public keys (where the public keys can be separated by commas or new lines).

**Example Input with Votes Specified Directly**
```shell
lisk> create transaction cast vote --votes 669efbe70b10c6c5d2b45465b0cb1e96edc66130a01de199185e5dba5da5aac0,215b667a32a5cd51a94c9c2046c11fffb08c65748febec099451e3b164452bca
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
```

**Example Input with Votes and Unvotes Specified Via a File**
```shell
lisk> create transaction cast vote --votes file:/path/to/votes.txt --unvotes file:/path/to/unvotes.txt
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
```

**Example JSON Output**
```json
{
	"amount": "0",
	"recipientId": "12475940823804898745L",
	"senderPublicKey": "a4465fd76c16fcc458448076372abf1912cc5b150663a64dffefe550f96feadd",
	"timestamp": 64793558,
	"type": 3,
	"fee": "100000000",
	"asset": {
		"votes": [
			"+669efbe70b10c6c5d2b45465b0cb1e96edc66130a01de199185e5dba5da5aac0",
			"+215b667a32a5cd51a94c9c2046c11fffb08c65748febec099451e3b164452bca"
		]
	},
	"signature": "1d45d794fbf78d0bec828b6876568cbbdc5cfb70eeb0c46d5278771c9db7fcb9fa3c80fb38c57bb57619ea6bb216dbcf7986afc5532dbf52e640407fbf7b6802",
	"id": "12646851302759999136"
}
```

## Create transaction create multisignature account

This command creates and signs a type 4 transaction, which will register the account as a multisignature account if broadcast to the network. The command takes three required parameters:

- A lifetime integer (the maximum number of hours within which a transaction from the multisignature account can be signed).
- A minimum number of signatures required for the transaction to be processed.
- One or more public keys to be included as part of the multisignature group.

**Example Input**
```shell
lisk> create transaction register multisignature account 24 2 215b667a32a5cd51a94c9c2046c11fffb08c65748febec099451e3b164452bca 922fbfdd596fa78269bbcadc67ec2a1cc15fc929a19c462169568d7a3df1a1aa
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
```

**Example JSON Output**
```json
{
	"amount": "0",
	"recipientId": "",
	"senderPublicKey": "a4465fd76c16fcc458448076372abf1912cc5b150663a64dffefe550f96feadd",
	"timestamp": 64793668,
	"type": 4,
	"fee": "1500000000",
	"asset": {
		"multisignature": {
			"min": 2,
			"lifetime": 24,
			"keysgroup": [
				"+215b667a32a5cd51a94c9c2046c11fffb08c65748febec099451e3b164452bca",
				"+922fbfdd596fa78269bbcadc67ec2a1cc15fc929a19c462169568d7a3df1a1aa"
			]
		}
	},
	"signature": "e82cf02e51db6d815fc1d2e0fa33099e1662f7e463d628d060a5155446e8497266260b00868fba8c61faf140291e9be9826401338bf80a74739e0f4ccca47209",
	"id": "17552046565394161055"
}
```

## Sign transaction

This command signs transaction. You will need the passphrase you sign with.

Since the secret passphrase is a sensitive input, it can be provided using one of the available methods described in the Sensitive Inputs section. The encrypted message can be provided either directly as an argument, or by specifying a source with the --message option. If both the secret passphrase and the encrypted message are provided via stdin, the secret passphrase must be given in the first line and the encrypted message must be given in the subsequent lines.


**Example Input with Argument**
```shell
lisk> sign transaction '{"amount":"100","recipientId":"13356260975429434553L","senderPublicKey":null,"timestamp":52871598,"type":0,"fee":"10000000","recipientPublicKey":null,"asset":{}}'
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
```

**Example JSON Output**
```json
{
	"amount": "10000000000",
	"recipientId": "13356260975429434553L",
	"senderPublicKey": "a4465fd76c16fcc458448076372abf1912cc5b150663a64dffefe550f96feadd",
	"timestamp": 64872831,
	"type": 0,
	"fee": "10000000",
	"recipientPublicKey": null,
	"asset": {},
	"signature": "0700e70310e4cd4fcb2bb1ec7527b760cc60f90b2629e270ecebd70affb8f6be3c163eda29e8e5ccedcae5f3401e7afd40244a30217aa0d435e762c874a63f00",
	"id": "15637644919032010963"
}
```

## Verify transaction

This command verify a transaction after being signed with the sign transaction command or create transaction command. You may specify second public key if the transaction has second signature.


**Example Input with Argument**
```shell
lisk> verify transaction '{"type":0,"amount":"100",...}' --second-public-key 647aac1e2df8a5c870499d7ddc82236b1e10936977537a3844a6b05ea33f9ef6
```

**Example Input with File Source Option**
```shell
lisk> verify transaction '{"type":0,"amount":"100",...}' --second-public-key file:/path/to/my/message.txt
```

**Example JSON Output**
```json
{
	"verified": true
}
```

## Broadcast transaction

This command broadcast transaction to the network. The command takes one required parameters:

- transaction as string in JSON format

**Example Input**
```shell
lisk> broadcast transaction '{"type":0,"amount":"100",...}'
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
```

**Example JSON Output**
```json
{
	"meta": {
		"status": true
	},
	"data": {
		"message": "Transaction(s) accepted"
	},
	"links": {}
}
```

## Broadcast signature

This command broadcast signature to the network. The command takes one required parameters:

- transaction as string in JSON format

**Example Input**
```shell
lisk> broadcast signature '{"transactionId":"abcd1234","publicKey":"abcd1234","signature":"abcd1234"}'
```

**Example JSON Output**
```json
{
	"meta": {
		"status": true
	},
	"data": {
		"message": "Signature(s) accepted"
	},
	"links": {}
}
```

## Sign message

This command signs message. You will need the passphrase you sign with.

Since the secret passphrase is a sensitive input, it can be provided using one of the available methods described in the Sensitive Inputs section. The encrypted message can be provided either directly as an argument, or by specifying a source with the --message option. If both the secret passphrase and the encrypted message are provided via stdin, the secret passphrase must be given in the first line and the encrypted message must be given in the subsequent lines.


**Example Input with Argument**
```shell
lisk> sign message 'Hello world'
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
```

**Example Input with File Source Option**
```shell
lisk> sign message --message file:/path/to/message.txt
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
```

**Example Input with Stdin Option (Non-Interactive Mode)**
```shell
 $ echo 'Hello World' | lisk sign message --message stdin
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
```

**Example JSON Output**
```json
{
	"message": "Hello World",
	"publicKey": "a4465fd76c16fcc458448076372abf1912cc5b150663a64dffefe550f96feadd",
	"signature": "0c70c0ed6ca16312c6acab46dd8b801fd3f3a2bd68018651c2792b40a7d1d3ee276a6bafb6b4185637edfa4d282e18362e135c5e2cf0c68002bfd58307ddb30b"
}
```

## Verify message

This command verify a message after being signed with the sign message command. You will need the public key, signature and message.


**Example Input with Argument**
```shell
lisk> verify message 647aac1e2df8a5c870499d7ddc82236b1e10936977537a3844a6b05ea33f9ef6 2a3ca127efcf7b2bf62ac8c3b1f5acf6997cab62ba9fde3567d188edcbacbc5dc8177fb88d03a8691ce03348f569b121bca9e7a3c43bf5c056382f35ff843c09 'Hello world'
```

**Example Input with File Source Option**
```shell
lisk> verify message 647aac1e2df8a5c870499d7ddc82236b1e10936977537a3844a6b05ea33f9ef6 2a3ca127efcf7b2bf62ac8c3b1f5acf6997cab62ba9fde3567d188edcbacbc5dc8177fb88d03a8691ce03348f569b121bca9e7a3c43bf5c056382f35ff843c09 --message file:/path/to/signed_message.txt
```

**Example Input with Stdin Option (Non-Interactive Mode)**
```shell
 $ echo 'Hello World' | lisk sign message 647aac1e2df8a5c870499d7ddc82236b1e10936977537a3844a6b05ea33f9ef6 2a3ca127efcf7b2bf62ac8c3b1f5acf6997cab62ba9fde3567d188edcbacbc5dc8177fb88d03a8691ce03348f569b121bca9e7a3c43bf5c056382f35ff843c09 --message stdin
```

**Example JSON Output**
```json
 {
	"verified": true
}
```

## Encrypt message

This command uses lisk-elements function to encrypt a message you provide for a given public key using a randomly generated nonce. In order to decrypt the encrypted message later your recipient will need your public key (to verify the message came from you), the nonce and the secret passphrase which matches the specified public key.

Since the password is a sensitive input, it can be provided using one of the available methods described in the Sensitive Inputs section. The message (which may also be a sensitive input) can be provided either directly as an argument (if security is not a concern), or by specifying a source with the --passphrase option. If both the password and the message are provided via stdin, the password must be given in the first line and the message must be given in the subsequent lines.

**Example Input with Argument**
```shell
lisk> encrypt message 5d036a858ce89f844491762eb89e2bfbd50a4a0a0da658e4b2628b25b117ae09 "My very secret message"
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
```

**Example Input with File Source Option**
```shell
lisk> encrypt message 5d036a858ce89f844491762eb89e2bfbd50a4a0a0da658e4b2628b25b117ae09 --message file:/path/to/message.txt
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
```

**Example Input with Stdin Option (Non-Interactive Mode)**
```shell
 $ echo "My very secret message" | lisk encrypt message 5d036a858ce89f844491762eb89e2bfbd50a4a0a0da658e4b2628b25b117ae09 --message stdin
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
```

**Example JSON Output**
```json
 {
	"nonce": "cb4d497e6834e0e888e285f32ddb02bdfd4b471f6ad04e6d",
	"encryptedMessage": "82af57f715c69958bda8b9e95b7f7a09bfaa5afeb94960bf243d7c77a656a3e1ff061c68e20e"
}
```

## Decrypt message

This command decrypts a message after being encrypted with the encrypt message command. You will need the sender's public key (to verify the message came from the sender), the secret passphrase which matches the public key used to encrypt the message, as well as the nonce which was randomly generated at the time of encryption.

Since the secret passphrase is a sensitive input, it can be provided using one of the available methods described in the Sensitive Inputs section. The encrypted message can be provided either directly as an argument, or by specifying a source with the --message option. If both the secret passphrase and the encrypted message are provided via stdin, the secret passphrase must be given in the first line and the encrypted message must be given in the subsequent lines.

**Example Input with Argument**
```shell
lisk> decrypt message bba7e2e6a4639c431b68e31115a71ffefcb4e025a4d1656405dfdcd8384719e0 1f9008c2813901366f3452431c27218be2c08ac85d6b28a3 f359abaf52a8fb68086cee580ce2b4656840c7c2af1308424eb9ff2b17eae87943502b8f14b6
Please enter your secret passphrase: *******
```

**Example Input with File Source Option**
```shell
lisk> decrypt message bba7e2e6a4639c431b68e31115a71ffefcb4e025a4d1656405dfdcd8384719e0 1f9008c2813901366f3452431c27218be2c08ac85d6b28a3 --message file:/path/to/encrypted_message.txt
Please enter your secret passphrase: ******
```

**Example Input with Stdin Option (Non-Interactive Mode)**
```shell
 $ echo f359abaf52a8fb68086cee580ce2b4656840c7c2af1308424eb9ff2b17eae87943502b8f14b6 | lisk decrypt message bba7e2e6a4639c431b68e31115a71ffefcb4e025a4d1656405dfdcd8384719e0 1f9008c2813901366f3452431c27218be2c08ac85d6b28a3 --message stdin
Please enter your secret passphrase: ******
```

**Example JSON Output**
```json
{
	"message": "My very secret message"
}
```

## Encrypt passphrase

This command uses AES-256-CBC to encrypt your secret passphrase under a password you provide using a randomly generated initialisation vector (IV). In order to decrypt the secret passphrase later you will need both the IV and the password.

Since the secret passphrase and password are both sensitive inputs, they can be provided using one of the available methods described in the Sensitive Inputs section. If both the secret passphrase and the password are provided via stdin, the secret passphrase must be given in the first line and the password must be given in the second line.

**Example Input**
```shell
lisk> encrypt passphrase
Please enter your secret passphrase: *******
Please re-enter your secret passphrase: *******
Please enter your password: *******
Please re-enter your password: *******
```

**Example JSON Output**
```json
{
	"encryptedPassphrase": "salt=11d8eccdc867a7a32f026f2f2dc624c8&cipherText=bc71fc&iv=997220e9861cd19bb9fc2441&tag=15e948552e9ada52186c2e9366754a34&version=1"
}
```

## Decrypt passphrase

This command decrypts your secret passphrase after being encrypted with the encrypt passphrase command. You will need the password you used to encrypt the secret passphrase as well as the initialisation vector (IV) which was randomly generated at the time of encryption.

Since the password is a sensitive input, it can be provided using one of the available methods described in the Sensitive Inputs section. The encrypted passphrase can be provided either directly as an argument, or by specifying a source with the --passphrase option. If both the password and the encrypted passphrase are provided via stdin, the password must be given in the first line and the encrypted passphrase must be given in the second line.

**Example Input with Argument**
```shell
lisk> decrypt passphrase salt=25606e160df4ababae0a0bd656310d7f&cipherText=4f513208f47dc539f7&iv=a048b9c1176b561a2f884f19&tag=2ef4db8d5e03c326fc26c0a8aa7adb69&version=1
Please enter your password: *******
```

**Example Input with File Source Option**
```shell
lisk> decrypt passphrase salt=25606e160df4ababae0a0bd656310d7f&cipherText=4f513208f47dc539f7&iv=a048b9c1176b561a2f884f19&tag=2ef4db8d5e03c326fc26c0a8aa7adb69&version=1 --passphrase file:./path/to/encrypted_passphrase.txt
Please enter your password: *******
```

**Example Input with Stdin Option (Non-Interactive Mode)**
```shell
 $ echo testing123 | lisk decrypt passphrase salt=25606e160df4ababae0a0bd656310d7f&cipherText=4f513208f47dc539f7&iv=a048b9c1176b561a2f884f19&tag=2ef4db8d5e03c326fc26c0a8aa7adb69&version=1 --passphrase stdin
Please enter your password: *******
```

**Example JSON Output**
```json
{
	"passphrase": "minute omit local rare sword knee banner pair rib museum shadow juice"
}
```

## Exit

This command exits the Lisk Commander terminal when in interactive mode.

**Input**
```shell
lisk> exit
```

## Get

This command gets information from the Lisk Blockchain.

**Available types:**

- account: gets information for a Lisk account by address
- address: alias for account
- block: gets information for a block by id
- delegate: gets information for a Lisk delegate by username
- transaction: gets information for a transaction by id

**Example Input**
```shell
lisk> get transaction 16388447461355055139
```

**Example JSON Output**
```json
{
	"id": "1963886604239976995",
	"height": 1,
	"blockId": "12584524832111619342",
	"type": 2,
	"timestamp": 0,
	"senderPublicKey": "1644013e59e410c547fda5dc49d9e0c0bc470f83cdbf845d18b320030c08df23",
	"senderId": "2676090374023786863L",
	"recipientId": "",
	"recipientPublicKey": "",
	"amount": "0",
	"fee": "0",
	"signature": "aba7721a9c77f06fb7018324196a93374779bbbb06ca223896122db234fcc8673a9bc802cca267eb3a4198ea885038ee0e063957f8a028463f9cbf4b17f15005",
	"signatures": [],
	"confirmations": 476185,
	"asset": {
		"delegate": {
			"username": "genesis_49",
			"publicKey": "1644013e59e410c547fda5dc49d9e0c0bc470f83cdbf845d18b320030c08df23",
			"address": "2676090374023786863L"
		}
	}
}
```

## Help

This command displays information for all the available commands.

**Input**
```shell
lisk> help
```

**Output**
```
  Commands:

    help [command...]                                                                                 Provides help for a given command.
    exit                                                                                              Exits Lisk Commander.
    broadcast signature [options] [signature]                                                         Broadcasts a signature to the network via the node
                                                                                                      specified in the current config. Accepts a stringified JSON signature as an
                                                                                                      argument, or a signature can be piped from a previous command. If piping in
                                                                                                      non-interactive mode make sure to quote out the entire command chain to avoid
                                                                                                      piping-related conflicts in your shell.

                                                                                                      Examples:
                                                                                                      - Interactive mode:
                                                                                                      - broadcast signature '{"transactionId":"abcd1234","publicKey":"abcd1234","signature":"abcd1234"}'
                                                                                                      - sign transaction '{"type":0,"amount":"100",...}' --json | broadcast signature
                                                                                                      - Non-interactive mode:
                                                                                                      - lisk "sign transaction '{"type":0,"amount":"100",...}' --json | broadcast signature"

    broadcast transaction [options] [transaction]                                                     Broadcasts a transaction to the network via the node
                                                                                                      specified in the current config. Accepts a stringified JSON transaction as an
                                                                                                      argument, or a transaction can be piped from a previous command. If piping in
                                                                                                      non-interactive mode make sure to quote out the entire command chain to avoid
                                                                                                      piping-related conflicts in your shell.

                                                                                                      Examples:
                                                                                                      - Interactive mode:
                                                                                                      - broadcast transaction '{"type":0,"amount":"100",...}'
                                                                                                      - create transaction transfer 100 13356260975429434553L --json | broadcast transaction
                                                                                                      - Non-interactive mode:
                                                                                                      - lisk "create transaction transfer 100 13356260975429434553L --json | broadcast transaction"

    config [options]                                                                                  Prints the current configuration.

                                                                                                      Example: config

    create account [options]                                                                          Returns a randomly-generated mnemonic passphrase with its corresponding public key and address.

                                                                                                      Example: create account

    create transaction cast votes [options]                                                           Creates a transaction which will cast votes (or unvotes) for delegate candidates using their public keys if broadcast to the network.

                                                                                                      Examples:
                                                                                                      - create transaction cast votes --votes 215b667a32a5cd51a94c9c2046c11fffb08c65748febec099451e3b164452bca,922fbfdd596fa78269bbcadc67ec2a1cc15fc929a19c462169568d7a3df1a1aa --unvotes e01b6b8a9b808ec3f67a638a2d3fa0fe1a9439b91dbdde92e2839c3327bd4589,ac09bc40c889f688f9158cca1fcfcdf6320f501242e0f7088d52a5077084ccba
                                                                                                      - create transaction 3 --votes 215b667a32a5cd51a94c9c2046c11fffb08c65748febec099451e3b164452bca,922fbfdd596fa78269bbcadc67ec2a1cc15fc929a19c462169568d7a3df1a1aa --unvotes e01b6b8a9b808ec3f67a638a2d3fa0fe1a9439b91dbdde92e2839c3327bd4589,ac09bc40c889f688f9158cca1fcfcdf6320f501242e0f7088d52a5077084ccba

    create transaction register delegate [options] <username>                                         Creates a transaction which will register the account as a delegate candidate if broadcast to the network.

                                                                                                      Examples:
                                                                                                      - create transaction register delegate username
                                                                                                      - create transaction 2 username

    create transaction register multisignature account [options] <lifetime> <minimum> <keysgroup...>  Creates a transaction which will register the account as a multisignature account if broadcast to the network, using the following parameters:
                                                                                                      - The lifetime (the number of hours in which the transaction can be signed after being created).
                                                                                                      - The minimum number of distinct signatures required for a transaction to be successfully approved from the multisignature account.
                                                                                                      - A list of one or more public keys that will identify the multisignature group.

                                                                                                      Examples:
                                                                                                      - create transaction register multisignature account 24 2 215b667a32a5cd51a94c9c2046c11fffb08c65748febec099451e3b164452bca 922fbfdd596fa78269bbcadc67ec2a1cc15fc929a19c462169568d7a3df1a1aa
                                                                                                      - create transaction 4 24 2 215b667a32a5cd51a94c9c2046c11fffb08c65748febec099451e3b164452bca 922fbfdd596fa78269bbcadc67ec2a1cc15fc929a19c462169568d7a3df1a1aa

    create transaction register second passphrase [options]                                           Creates a transaction which will register a second passphrase for the account if broadcast to the network.

                                                                                                      Examples:
                                                                                                      - create transaction register second passphrase
                                                                                                      - create transaction 1

    create transaction transfer [options] <amount> <address>                                          Creates a transaction which will transfer the specified amount to an address if broadcast to the network.

                                                                                                      Examples:
                                                                                                      - create transaction transfer 100 13356260975429434553L
                                                                                                      - create transaction 0 100 13356260975429434553L

    decrypt message [options] <senderPublicKey> <nonce> [message]                                     Decrypts a previously encrypted message from a given sender public key for a known nonce using your secret passphrase.

                                                                                                      Example: decrypt message bba7e2e6a4639c431b68e31115a71ffefcb4e025a4d1656405dfdcd8384719e0 349d300c906a113340ff0563ef14a96c092236f331ca4639 e501c538311d38d3857afefa26207408f4bf7f1228

    decrypt passphrase [options] [encryptedPassphrase]                                                Decrypts your secret passphrase using a password using the initialisation vector (IV) which was provided at the time of encryption.

                                                                                                      Example: decrypt passphrase salt=25606e160df4ababae0a0bd656310d7f&cipherText=4f513208f47dc539f7&iv=a048b9c1176b561a2f884f19&tag=2ef4db8d5e03c326fc26c0a8aa7adb69&version=1

    encrypt message [options] <recipient> [message]                                                   Encrypts a message for a given recipient public key using your secret passphrase.

                                                                                                      Example: encrypt message bba7e2e6a4639c431b68e31115a71ffefcb4e025a4d1656405dfdcd8384719e0 'Hello world'

    encrypt passphrase [options]                                                                      Encrypts your secret passphrase under a password.

                                                                                                      Example: encrypt passphrase

    get [options] <type> <input>                                                                      Gets information from the blockchain. Types available: account, address, block, delegate, transaction.

                                                                                                      Examples:
                                                                                                      - get delegate lightcurve
                                                                                                      - get block 5510510593472232540

    list [options] <type> <inputs...>                                                                 Gets an array of information from the blockchain. Types available: accounts, addresses, blocks, delegates, transactions.

                                                                                                      Examples:
                                                                                                      - list delegates lightcurve tosch
                                                                                                      - list blocks 5510510593472232540 16450842638530591789

    set [options] <variable> [values...]                                                              Sets configuration <variable> to [value(s)...]. Variables available: api.nodes, api.network, json, name, pretty. Configuration is persisted in `/Users/shuse2/.lisk-commander/config.json`.

                                                                                                      Examples:
                                                                                                      - set json true
                                                                                                      - set name my_custom_lisk_cli
                                                                                                      - set api.network main
                                                                                                      - set api.nodes https://127.0.0.1:4000,http://mynode.com:7000

    show account [options]                                                                            Shows account information for a given passphrase.

                                                                                                      Example: show account

    show copyright [options]                                                                          Displays copyright notice.

                                                                                                      Example: show copyright

    show warranty [options]                                                                           Displays warranty notice.

                                                                                                      Example: show warranty

    sign message [options] [message]                                                                  Sign a message using your secret passphrase.

                                                                                                      Example: sign message 'Hello world'

    sign transaction [options] [transaction]                                                          Sign a transaction using your secret passphrase.

                                                                                                      Example: sign transaction '{"amount":"100","recipientId":"13356260975429434553L","senderPublicKey":null,"timestamp":52871598,"type":0,"fee":"10000000","recipientPublicKey":null,"asset":{}}'

    verify message [options] <publicKey> <signature> [message]                                        Verify a message using the public key, the signature and the message.

                                                                                                      Example: verify message 647aac1e2df8a5c870499d7ddc82236b1e10936977537a3844a6b05ea33f9ef6 2a3ca127efcf7b2bf62ac8c3b1f5acf6997cab62ba9fde3567d188edcbacbc5dc8177fb88d03a8691ce03348f569b121bca9e7a3c43bf5c056382f35ff843c09 'Hello world'

    verify transaction [options] [transaction]                                                        Verifies a transaction has a valid signature.

                                                                                                      Examples:
                                                                                                      - verify transaction '{"type":0,"amount":"100",...}'
                                                                                                      - verify transaction '{"type":0,"amount":"100",...}' --second-public-key 647aac1e2df8a5c870499d7ddc82236b1e10936977537a3844a6b05ea33f9ef6
                                                                                                      - create transaction transfer 100 123L --json | verify transaction
```

The help command can also be used to get specific information for a single command.

**Example Input**
```shell
lisk> help config
```

**Example Output**
```
 Usage: config [options]

  Alias: env

  Prints the current configuration.

	Example: config

  Options:

    --help         output usage information
    -j, --json     Sets output to json. You can change the default behaviour in your config.json file.
    -t, --no-json  Sets output to text (default). You can change the default behaviour in your config.json file.
    --pretty       Prints json in pretty format rather than condensed. Has no effect if json option is false. You can change the default behaviour in your config.json file.
```

## List

This command is the variadic form of the get command. You can retrieve information for multiple transactions, delegates etc. at once. Inputs should be separated by a space.

**Available types:**

- accounts: gets information for Lisk accounts by address
- addresses: alias for accounts
- blocks: gets information for blocks by id
- delegates: gets information for Lisk delegates by username
- transactions: gets information for transactions by id

**Example Input**
```shell
lisk> list delegates genesis_1 genesis_5
```

**Example JSON Output**
```json
[
	{
		"rewards": "2364000000000",
		"vote": "9659948140000000",
		"producedBlocks": 4730,
		"missedBlocks": 163,
		"username": "genesis_1",
		"rank": 83,
		"approval": 94.36,
		"productivity": 96.67,
		"account": {
			"address": "17956766617869547133L",
			"publicKey": "b8f2cce407a6c88142bce6d79a4fe9d73f0e6218a8f44dd939144c64d1c1e1ee",
			"secondPublicKey": ""
		}
	},
	{
		"rewards": "2381500000000",
		"vote": "9659948140000000",
		"producedBlocks": 4765,
		"missedBlocks": 151,
		"username": "genesis_5",
		"rank": 55,
		"approval": 94.36,
		"productivity": 96.93,
		"account": {
			"address": "12108854850941227449L",
			"publicKey": "3c9acbe6789f806d097893e769a058aa9b8ebd6ca9d8bf07989f4d27a9a2b4d5",
			"secondPublicKey": ""
		}
	}
]
```

## Set

This command lets you manipulate the config.json file via the command line. These settings will then be used as defaults, but you can still override these defaults with command line options. Changes are reflected immediately in the Lisk Commander terminal if run in interactive mode.

**Example Input**
```shell
lisk> set api.network main
```

**Example JSON Output**
```json
{
	"message": "Successfully set api.network to main."
}
```
