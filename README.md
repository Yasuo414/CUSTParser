# C++ USTParser Library Documentation

The USTParser is a C++ version of my Python library designed to parse UST (UTAU Sequence Text) files, which are used in the UTAU singing voice synthesizer software. This library provides functionality to read UST files, extract note information, and retrieve phonemes, durations, and pitch sequences.

## Table of Contents

1. [Installation](#installation)
2. [Library Structure](#library-structure)
3. [Usage](#usage)
   - [Including the Library](#including-the-library)
   - [Parsing a UST File](#parsing-a-ust-file)
   - [Accessing Parsed Data](#accessing-parsed-data)
   - [Retrieving Phonemes and Durations](#retrieving-phonemes-and-durations)
   - [Retrieving Pitch Sequence](#retrieving-pitch-sequence)
4. [Example](#example)
5. [API Reference](#api-reference)
   - [Note Class](#note-class)
   - [Parser Class](#parser-class)

## Installation

To use this library in your C++ project:

1. Copy `USTParser.hpp` and `USTParser.cpp` into your project directory.
2. Include `USTParser.hpp` in your source files where you want to use the library.
3. Compile `USTParser.cpp` along with your other source files.

## Library Structure

The library consists of two main files:

- `USTParser.hpp`: Header file containing class declarations.
- `USTParser.cpp`: Implementation file containing the function definitions.

## Usage

### Including the Library

In your C++ source files, include the header file:

```cpp
#include "USTParser.hpp"
```

### Parsing a UST File

To parse a UST file, create an instance of the `Parser` class with the file path:

```cpp
Parser parser("path/to/your/ust/file.ust");
```

### Accessing Parsed Data

After parsing, you can access the following methods:

```cpp
// Get the project name
std::string project_name = parser.getProjectName();

// Get the global tempo
float tempo = parser.getTempo();

// Get all notes
const std::vector<Note>& notes = parser.getNotes();
```

### Retrieving Phonemes and Durations

To get the phonemes and their corresponding durations:

```cpp
std::vector<float> durations;
std::vector<std::string> phonemes = parser.getPhonemesAndDurations(durations);
```

### Retrieving Pitch Sequence

To get the sequence of pitches (as MIDI note numbers):

```cpp
std::vector<int> pitch_sequence = parser.getPitchSequence();
```

## Example

Here's a complete example demonstrating how to use the USTParser library:

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

## API Reference

### Note Class

The `Note` class represents a single note in the UST file.

#### Members

- `std::string lyric`: The lyric or phoneme of the note.
- `int length`: The length of the note.
- `int notenum`: The MIDI note number.
- `float tempo`: The tempo of the note (default is 120.0).

#### Constructor

```cpp
Note(const std::string& l, int len, int nn, float t = 120.0);
```

Creates a new `Note` instance with the given lyric, length, note number, and tempo.

### Parser Class

The `Parser` class is responsible for parsing the UST file and providing access to the parsed data.

#### Constructor

```cpp
explicit Parser(const std::string& path);
```

Creates a new `Parser` instance and parses the UST file at the given path.

#### Public Methods

```cpp
std::vector<std::string> getPhonemesAndDurations(std::vector<float>& durations);
```
Returns a vector of phonemes and fills the provided `durations` vector with corresponding durations.

```cpp
std::vector<int> getPitchSequence();
```
Returns a vector of MIDI note numbers representing the pitch sequence.

```cpp
const std::string& getProjectName() const;
```
Returns the project name.

```cpp
float getTempo() const;
```
Returns the global tempo of the project.

```cpp
const std::vector<Note>& getNotes() const;
```
Returns a const reference to the vector of parsed notes.

This documentation provides a comprehensive guide to using the C++ USTParser library, including installation instructions, usage examples, and an API reference. Users can refer to this documentation to understand how to integrate and utilize the library in their C++ projects.
