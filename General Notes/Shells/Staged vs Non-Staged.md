## Staged vs Non-Staged Payloads
There are two main types of payloads to pay attention to:
1. Non-staged
2. Staged

### Non-Staged:
-Sends exploit shellcode all at once
-Larger in size, won't always work
-windows/**meterpreter_reverse_tcp**

### Staged:
-Sends payload in stages
-Less stable
-windows/**meterpreter/reverse_tcp**

So if you have a payload that doesn't work, then try a smaller, staged payload, and vice versa.
Sometimes you have to just troubleshoot with different payloads, bind and reverse, to see what works.