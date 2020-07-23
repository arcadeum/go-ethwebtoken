```
 ____ ____ ____ ____ ____ ____ ____ ____ ____ ____ ____ 
||e |||t |||h |||w |||e |||b |||t |||o |||k |||e |||n ||
||__|||__|||__|||__|||__|||__|||__|||__|||__|||__|||__||
|/__\|/__\|/__\|/__\|/__\|/__\|/__\|/__\|/__\|/__\|/__\|
```

## Format

`ewt = eth.<address>.<claims>.<signature>`


### Address

The account address in hex encoding, ie. '0x9e63b5BF4b31A7F8d5D8b4f54CD361344Eb744C5'.

Note, you should not take the account address in the ewt at face value -- you must parse the EWT
and validate it with the library methods provided. The address is included when used to verify
smart wallet based accounts (aka contract-based accounts).


### Claims

a base64 encoded JSON object of

```typescript
interface EWTClaims {
  app: string
  iat: number
  exp: number
  n?: number
  typ?: string
  ogn?: string
}
```

Fields:

  * `app` (required) - App identifier requesting the issuance of the token
  * `iat` (required) - Issued at unix timestamp of when the token has been signed/issued
  * `exp` (required) - Expired at unix timestamp of when the token is valid until
  * `n` (optional) - Nonce value which can be used as a challenge number for added security
  * `typ` (optional) - Type of authorization for this ewt
  * `ogn` (optional) - Domain origin requesting the issuance of the token


### Signature

Signature value of the claims message payload. The signature is computed by the EIP712
eth_signTypedData call of the claims object. The signature may be recoverable with ECRecover to
determine the EOA address, or you may have a different encoding such as one used with EIP-1271,
to validate the contract-based account signature.


## HTTP Authorization Header

EWT's can be used similarly to JWT's, passed to the 'Authorization' header of a HTTP request.

The format of the header is:

`Authorization: Bearer <ewt>`

for example:

`Authorization: Bearer eth.0x8ec767428b824b39c307085e1b9f035464907d31.eyJpYXQiOjE1OTQ3NDMxODEsImV4cCI6MTU5NDc0MzQ4MX0.0x9070796b0ed4597fdfd3ed89c13f26422dd6375329939ab0aa0a65ddafbe10af1048a3bebfdf390127270b1955fa1301afaa10df98890b6a2924ef812acf2ab71c`


## Example EWT encoding / decoding

### EOA account signature

ewt = `eth.0x89d9f8f31817badb5d718cd6fb483b71dbd2dfed.eyJhcHAiOiJFV1RUZXN0IiwiaWF0IjoxNTk1NTMwODQwLCJleHAiOjE1OTU1MzExNDB9.0x233ab9164a677a41acc8d52c9e1d1a621acebf9bc8d956c8474618b589acebe10cc350deb4b02bf6951cec8bd23507170f204ca326a5a264b8f6f67fa2619c251c`

decodes & verifies to:
  * account address: `0x89D9F8f31817BAdb5D718CD6fb483b71DbD2dfeD`
  * claims: `{"app":"EWTTest","iat":1595530840,"exp":1595531140}`
  * signature: `0x233ab9164a677a41acc8d52c9e1d1a621acebf9bc8d956c8474618b589acebe10cc350deb4b02bf6951cec8bd23507170f204ca326a5a264b8f6f67fa2619c251c`


### Contract-based account signature (verifiable with EIP 1271)

ewt = `eth.0x9e63b5bf4b31a7f8d5d8b4f54cd361344eb744c5.eyJpYXQiOjE1OTQ3NDM4NDgsImV4cCI6MTYyNjI3OTg0OCwibiI6MTMzN30.0x000100012dd090aec5e4a9678f7968533c10fc42b07b9a23fa3b719f79a861adcfc7e1d958e3521bb061c34072f5435681390ccc9be19bf9da32320bd2356d0b4b4d316b1c02`

decodes & verifies to:
  * account address: `0x9e63b5bf4b31a7f8d5d8b4f54cd361344eb744c5`
  * message: `{"iat":1594743848,"exp":1626279848,"n":1337}`
  * signature: `0x000100012dd090aec5e4a9678f7968533c10fc42b07b9a23fa3b719f79a861adcfc7e1d958e3521bb061c34072f5435681390ccc9be19bf9da32320bd2356d0b4b4d316b1c02`


## LICENSE

MIT
