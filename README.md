# USTParser C++

The USTParser C++ is designed to parse UST (UTAU Sequence Text) files, which are used in the UTAU singing voice synthesizer software. This library provides functionality to read UST files, extract note information, and retrieve phonemes, durations, and pitch sequences.

## Installation

To use this library, include the header file in your C++ project.

## Usage

### Including the library

```cpp
#include "USTParser.hpp"
```

### Parsing a UST file

To parse a UST file, create an instance of the `Parser` class with the file path:

```cpp
Parser parser("path/to/your/ust/file.ust");
```

### Accessing parsed data

After parsing, you can access the following methods:

- `parser.getProjectName()`: Returns the name of the project (std::string)
- `parser.getTempo()`: Returns the global tempo of the project (float)
- `parser.getNotes()`: Returns a vector of `Note` objects representing each note in the UST file

### Retrieving phonemes and durations

To get the phonemes and their corresponding durations:

```cpp
std::vector<float> durations;
std::vector<std::string> phonemes = parser.getPhonemesAndDurations(durations);
```

### Retrieving pitch sequence

To get the sequence of pitches (as MIDI note numbers):

```cpp
std::vector<int> pitch_sequence = parser.getPitchSequence();
```

## Example

Here's a complete example of how to use the USTParser C++ library:

```cpp
#include <iostream>
#include <vector>
#include "USTParser.hpp"

int main() {
    try {
        // Parse the UST file
        Parser parser("example.ust");

        // Print project information
        std::cout << "Project Name: " << parser.getProjectName() << std::endl;
        std::cout << "Tempo: " << parser.getTempo() << " BPM" << std::endl;

        // Print information about each note
        const auto& notes = parser.getNotes();
        for (size_t i = 0; i < notes.size(); ++i) {
            const auto& note = notes[i];
            std::cout << "Note " << (i + 1) << ":" << std::endl;
            std::cout << "  Lyric: " << note.lyric << std::endl;
            std::cout << "  Length: " << note.length << std::endl;
            std::cout << "  Note Number: " << note.notenum << std::endl;
            std::cout << "  Tempo: " << note.tempo << std::endl;
        }

        // Get phonemes and durations
        std::vector<float> durations;
        std::vector<std::string> phonemes = parser.getPhonemesAndDurations(durations);
        
        std::cout << "Phonemes: ";
        for (const auto& phoneme : phonemes) {
            std::cout << phoneme << " ";
        }
        std::cout << std::endl;

        std::cout << "Durations: ";
        for (float duration : durations) {
            std::cout << duration << " ";
        }
        std::cout << std::endl;

        // Get pitch sequence
        std::vector<int> pitch_sequence = parser.getPitchSequence();
        std::cout << "Pitch sequence: ";
        for (int pitch : pitch_sequence) {
            std::cout << pitch << " ";
        }
        std::cout << std::endl;

    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
        return 1;
    }

    return 0;
}
```

This example demonstrates how to parse a UST file, access project information, iterate through notes, and retrieve phonemes, durations, and pitch sequences using the C++ USTParser library.
