# lempel-ziv-compression
A very slow python implimentation of the LZ78 compression algorithm.

## How do you use it?
Here is an example of a command to compress a file called data.txt, writing the result to data.txt.compressed:

    C:\YourDirectoryHere\lempel-ziv-compression> python lz78.py compress data.txt

Here is how you would decompress the file called data.txt.compressed:

    C:\YourDirectoryHere\lempel-ziv-compression> python lz78.py uncompress data.txt.compressed

## How does it work?
The LZ78 compression is a lossless compression algorithm published by Abraham Lempel and Jacob Ziv in 1978. It is unusual, in the way that it does not send a dictionary along with the compressed data, as one might expect. Instead, it generates the dictionary as the code is being compressed/decompressed, removing the need to send a dictionary with the compressed data. Its primary limitation is that, in order to uncompress data, the decompression must start at the first character. However, this can be overcome by using a partially complete premade dictionary (created by decompressing all the characters up to that point).

## How does it really work?
Here is an example compression of the string "AABABABA".
First, the algorithm starts of with a dictionary containing two items:

    dictionary = {String.EMPTY:0, FirstCharacterInString:1}
    output = string(FirstCharacterInString)                   // output is now "A"

Next, the algorithm starts the substring at the second character (index 1).

    currentPattern = ""
    for i = 0 to input.LENGTH:      // not inclusive
        currentPattern += input[i]        // currentPattern is now "A"

It then checks wether it has seen the substring before. In this case, it has (because the "A" is in the dictionary):

    if currentPattern in dictionary.keys:
        continue to the next loop

The next character in the input is appended to the current pattern (which is now "AB") and the program checks again.

    currentPattern += input[i]                  // currentPattern is now "AB"
    if currentPattern in dictionary.keys:       // return False
        continue to the next loop
    else:
        output += dictionary[currentPattern.untilLastLetter()]  // output += dictionary["A"] (1), output is now "A1"
        output += currentPattern.lastLetter()       // output += "B", output is now "A1B"
        dictionary[currentPattern] = len(dictionary)  // dictionary["AB"] is now 2
        currentPattern = ""   // reset the current pattern

This cycle continues, until the compressed string ends up as "A1B2A0B1". Decompression is very similar, except the output is used to reconstruct the dictionary, which is then used to reconstruct the original input.
