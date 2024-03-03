#### We found some cowboys sharing secrets online. Can you decode this?

1. inspected packet capture in wireshark
	1. looked at different services like dns, http, etc
	2. looked at the different conversations between devices
2. Broke down the prompt and title
	1. primitives are how you filter in wireshark
		1. https://www.tutorialspoint.com/what-are-the-service-primitives-in-computer-network
		2. https://www.wireshark.org/docs/wsug_html/
	2. realized I would need to find the right primitive to filter for, so I picked the longest conversation and started there
	3. Secrets are a part of tls and there were a lot of tls packets in the capture
		1. started looking into viewing secrets in wireshark
			1. https://www.netburner.com/learn/the-secret-is-out-how-to-view-encrypted-data-in-wireshark/
		2. Learned that I could probably find the secret to decrypt the tls traffic
			1. https://blog.didierstevens.com/2020/12/14/decrypting-tls-streams-with-wireshark-part-1/
				1. this doc was a good start, but not what I was looking for :/
3. Began search for the secret and related information
	1. 13918 - pub key
	2. 13911 - Cipher suite
	3. 12362 - new session ticket
		1. no dice??
	4. (packet 86) Client Random: 42ae1df6b2ed5f90d5517b53a62cd959a0e3fde8b7c91e4ef3919d2182d81289
		1. Was looking at this: https://blog.didierstevens.com/2020/12/28/decrypting-tls-streams-with-wireshark-part-2/
		2. said just the client random would be enough to decrypt (they lied)
	5. 
Transport Layer Security
    TLSv1.2 Record Layer: Handshake Protocol: Client Key Exchange
        Content Type: Handshake (22)
        Version: TLS 1.2 (0x0303)
        Length: 37
        Handshake Protocol: Client Key Exchange
            Handshake Type: Client Key Exchange (16)
            Length: 33
            EC Diffie-Hellman Client Params
                Pubkey Length: 32
                Pubkey: 3071aa68dde36c95ccab951edc7e600af3a39f300095bbdf062704de770f037b
