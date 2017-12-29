# Excuse me, Server, do you have the Time?

### Identifying timebased tokens hiding in plain sight.

# Abstract

A core component of many application protections is provided via the use of a unique, unpredictable, string of data that is commonly referred to as a token. Tokens generated using timebased values for entropy may be unrecognizable to the human eye, but can be identified by comparision of token samples collected in quick succession.

## Shortcoming of Statistical Analysis

There are tools available to help with the collection and analysis of tokens. One notable tool is the [Sequencer][3] tool inside of [PortSwiggers Burp Suite][4]. The tool peforms statistical analysis of tokens. The tool warns about the limitations of statistical analysis. The following is an excerpt from the [tools documentation][3].

>Data that is generated in a completely deterministic way may be deemed to be random by statistical tests. For example, a well-designed linear congruential pseudo-random number generator, or an algorithm which computes the hash of a sequential number, may produce seemingly random output.

This condition has been observed with tokens generated from the digest of data containing time values. Statistical analysis alone is unreliable in determining whether these types of tokens are deterministic.

## Collection Method

The following is a technique to identify tokens that:
* Are generated from time-based data.
* Do not contain another source of entropy. 
* May or may not contain additional data.

Tokens requested in quick succession using multiple threads forces the application to spawn its own threads to respond to the requests. If token meets the criteria and uses the same time value for at least two tokens the application will eventually return the same token twice. 

This phenomenon has been observed on networks with varying latency against targets using timestamps with millisecond resolution. Timebased tokens may be able to be predicted under the right conditions and depending on its format.

## UUID

A [UUID][5] is an identifier that is unique across both space and time, with respect to the space of all UUIDs. UUIDs are 128 bits long and usually delimited by hyphens in the following format: 

>58e0a7d7-eebc-`1`1d8-9669-0800200c9a66

The highlighted portion represents the version of the UUID. There are 5 different versions of UUID. 
* Version 1 - Data-time and MAC address
* Version 2 - POSIX UID/GID and local domain (not explicitly defined in specification).
* Version 3 - MD5 hash of namespace, URL, FQDN, Object ID, Other
* Version 4 - Random/Pseduo-Random
* Version 5 - Version 3 using SHA1 hash.

The [specification explicitly states][5] not to use UUID to implement security capabilities. 

Version 1 UUIDs are derivied from a time value that represents a 60-bit count of 100-nanosecond intervals since 00:00:00.00, 15 October 1582 (first section highlighted). The final portion of the UUID is derivied from the MAC address(es) of the system (highlighted last). 

>`58e0a7d7-eebc`-11d8-9669-`0800200c9a66`

The system clock may not be able to support resolution of a 100-nanoseconds. Some implementations will increment the time value by number of UUIDs generated within a single milli-second.

### Generating UUIDs

Generating Version 1 UUIDs requires collecting a sample UUID from the application. The time value will be used as the starting point for the time value.
~~~~
        UUID seed_uuid = decodeUUID(seed_user_id);
    	long seed_date = seed_uuid.timestamp();
~~~~
The time stamp value is incremented postively or negatively. The standard calls for this value to be a representation of 100 nanoseconds. However, practical examples have shown that incrementing by milliseconds to be remarkably successful. This means it is very likely only a 1,000 candidates per second is needed to be generated to brute-force the valid values in the UUID space, as opposed to 1,000,000 candidates per second.
~~~~
    	seed_date = seed_date + increment;
~~~~
The timestamp is then converted from a Long representation to the representation of the UUID standard.
~~~~
    	byte[] timeBytes = longToBytes(seed_date);
    	String firstOct = bytesToHex(new byte[]{timeBytes[4], timeBytes[5], timeBytes[6], timeBytes[7]});
    	String secondOct = bytesToHex(new byte[]{timeBytes[2], timeBytes[3]});
    	String thirdOct = bytesToHex(new byte[]{timeBytes[0], timeBytes[1]}).replace("0", "1");
~~~~
Finally, the new time value is concatenated with the remaining suffix of our seed value to create our test candidate.
~~~~
    	String test_candidate = (firstOct + d + secondOct + d + thirdOct + d).toLowerCase() + suffix;
~~~~
This sample code is available on [GitHub][7].

## Message Digest

Message digest, commonly known as hashes, are usually represented in ascii-hex (0-9,A-F) and vary in length from 32 characters (128 bits) to 128 characters (512 bits). Message digests may also be represented in other encodings, such as Base64, and not immediately recognizable without decoding and coverting to ascii-hex. 

### Preimage Attack

Time-based message digests that are not derived from HMAC, which require knowledge of a secret key, are potential candidates for a preimage attack. This type of attack attempts to determine the the source data used to create the token. This attack will vary in success depending upon the complexity of the source data. Simple digests of epoch time may be trivial while complex concatenated values will be nearly impossible.

The server should be interegated for information leaks that provide information about the current server time. A commonly source is epoch time values appended to JavaScript and CSS resource links. The time representations for the programming language of the application should be incremented, hashed, and compared against the list or previously collected values. 

Additional guesses can be crafted from permutations of known data such as user IDs, role IDs, or source IP addresses. A proof of concept permutation generator is available [here][8].

## Cipher Texts
The collection method will be able to identify time-based data inside of certain cipher texts, but without knowledge of the encryption key or initilization vector it is poor candidate for predication. 


# References

* A Universally Unique IDentifier (UUID) URN Namespace. https://www.ietf.org/rfc/rfc4122.txt
* Burp Sequencer Documentation.  https://portswigger.net/burp/help/sequencer.html
* The Cost of GUIDs as Primary Keys; Nilsson, Jimmy (2002). http://www.informit.com/articles/article.aspx?p=25862&seqNum=7
* Universally unique identifier. https://en.wikipedia.org/wiki/Universally_unique_identifier


[3]: https://portswigger.net/burp/help/sequencer_tests.html "Burp Sequencer Documentation"
[4]: https://portswigger.net/burp/
[5]: https://www.ietf.org/rfc/rfc4122.txt
[6]: http://www.informit.com/articles/article.aspx?p=25862&seqNum=7
[7]: https://github.com/bcardinale/TimeCrunch/blob/master/src/mhat/sands/generator/UUIDGenerator.java
[8]: https://github.com/bcardinale/TimeCrunch/blob/master/src/mhat/sands/generator/PermutationGenerator.java
[9]: https://twitter.com/brian_cardinale
[10]: https://www.linkedin.com/in/brian-cardinale-cissp-51256833

#### Metadata

Tags: uuid, guid, tokens, session

* **Primary Author Name**: [Brian Cardinale][10]
* **Primary Author Affiliation**: www.cardinaleconcepts.com | [@brian_cardinale][9]
