## **Understanding the Adapter Design Pattern with Tons of Code Samples (TypeScript & PHP)**

### **What is the Adapter Pattern?**
The **Adapter Pattern** is a structural design pattern that allows objects with incompatible interfaces to work together. It acts as a bridge between two interfaces, making them compatible without altering their actual implementations.

---

## **Breaking It Down Step by Step**
To help you fully understand the Adapter Pattern, let's go through:

1. **Basic Example**
2. **Real-World Use Cases**
   - Third-Party API Integration
   - Database Driver Adapter
   - Payment Gateway Adapter
   - Legacy System Adapter
3. **Bad Implementations of the Adapter Pattern**
4. **Summary**

---

# **1. Basic Example: Media Player Adapter**
Imagine you have a **modern media player** that plays **MP3** files, but now you need it to play **MP4 and VLC** formats without changing the existing code.

### **TypeScript Implementation**
```typescript
// Step 1: Define the Target Interface
interface MediaPlayer {
  play(fileName: string): void;
}

// Step 2: Existing MP3 Player (Already Working)
class Mp3Player implements MediaPlayer {
  play(fileName: string): void {
    console.log(`Playing MP3 file: ${fileName}`);
  }
}

// Step 3: Adaptee - New Advanced Media Player (Incompatible Interface)
class AdvancedMediaPlayer {
  playMp4(fileName: string): void {
    console.log(`Playing MP4 file: ${fileName}`);
  }

  playVlc(fileName: string): void {
    console.log(`Playing VLC file: ${fileName}`);
  }
}

// Step 4: Adapter that Converts AdvancedMediaPlayer to MediaPlayer
class MediaAdapter implements MediaPlayer {
  private advancedMediaPlayer: AdvancedMediaPlayer;

  constructor(advancedMediaPlayer: AdvancedMediaPlayer) {
    this.advancedMediaPlayer = advancedMediaPlayer;
  }

  play(fileName: string): void {
    if (fileName.endsWith(".mp4")) {
      this.advancedMediaPlayer.playMp4(fileName);
    } else if (fileName.endsWith(".vlc")) {
      this.advancedMediaPlayer.playVlc(fileName);
    } else {
      console.log("Invalid file format.");
    }
  }
}

// Step 5: Client Code
class AudioPlayer {
  private mediaPlayer: MediaPlayer;

  constructor(mediaPlayer: MediaPlayer) {
    this.mediaPlayer = mediaPlayer;
  }

  playFile(fileName: string): void {
    this.mediaPlayer.play(fileName);
  }
}

// Testing
const mp3Player = new AudioPlayer(new Mp3Player());
mp3Player.playFile("song.mp3");

const mp4Adapter = new AudioPlayer(new MediaAdapter(new AdvancedMediaPlayer()));
mp4Adapter.playFile("movie.mp4");
mp4Adapter.playFile("clip.vlc");
```

---

### **PHP Implementation**
```php
<?php

// Step 1: Define the Target Interface
interface MediaPlayer {
    public function play(string $fileName): void;
}

// Step 2: Existing MP3 Player (Already Working)
class Mp3Player implements MediaPlayer {
    public function play(string $fileName): void {
        echo "Playing MP3 file: " . $fileName . PHP_EOL;
    }
}

// Step 3: Adaptee - New Advanced Media Player (Incompatible Interface)
class AdvancedMediaPlayer {
    public function playMp4(string $fileName): void {
        echo "Playing MP4 file: " . $fileName . PHP_EOL;
    }

    public function playVlc(string $fileName): void {
        echo "Playing VLC file: " . $fileName . PHP_EOL;
    }
}

// Step 4: Adapter that Converts AdvancedMediaPlayer to MediaPlayer
class MediaAdapter implements MediaPlayer {
    private AdvancedMediaPlayer $advancedMediaPlayer;

    public function __construct(AdvancedMediaPlayer $advancedMediaPlayer) {
        $this->advancedMediaPlayer = $advancedMediaPlayer;
    }

    public function play(string $fileName): void {
        if (str_ends_with($fileName, ".mp4")) {
            $this->advancedMediaPlayer->playMp4($fileName);
        } elseif (str_ends_with($fileName, ".vlc")) {
            $this->advancedMediaPlayer->playVlc($fileName);
        } else {
            echo "Invalid file format." . PHP_EOL;
        }
    }
}

// Step 5: Client Code
class AudioPlayer {
    private MediaPlayer $mediaPlayer;

    public function __construct(MediaPlayer $mediaPlayer) {
        $this->mediaPlayer = $mediaPlayer;
    }

    public function playFile(string $fileName): void {
        $this->mediaPlayer->play($fileName);
    }
}

// Testing
$mp3Player = new AudioPlayer(new Mp3Player());
$mp3Player->playFile("song.mp3");

$mp4Adapter = new AudioPlayer(new MediaAdapter(new AdvancedMediaPlayer()));
$mp4Adapter->playFile("movie.mp4");
$mp4Adapter->playFile("clip.vlc");
```

---

# **2. Real-World Use Cases**
### **a) Third-Party API Integration**
You want to integrate a third-party weather API that provides temperature in **Fahrenheit**, but your system works with **Celsius**.

#### **TypeScript Adapter**
```typescript
interface WeatherService {
  getTemperature(city: string): number;
}

class ThirdPartyWeatherAPI {
  getTemperatureInFahrenheit(city: string): number {
    return 75; // Returns temperature in Fahrenheit
  }
}

class WeatherAdapter implements WeatherService {
  private api: ThirdPartyWeatherAPI;

  constructor(api: ThirdPartyWeatherAPI) {
    this.api = api;
  }

  getTemperature(city: string): number {
    return ((this.api.getTemperatureInFahrenheit(city) - 32) * 5) / 9;
  }
}

const weather = new WeatherAdapter(new ThirdPartyWeatherAPI());
console.log(weather.getTemperature("New York")); // Returns temperature in Celsius
```

#### **PHP Adapter**
```php
<?php
interface WeatherService {
    public function getTemperature(string $city): float;
}

class ThirdPartyWeatherAPI {
    public function getTemperatureInFahrenheit(string $city): float {
        return 75;
    }
}

class WeatherAdapter implements WeatherService {
    private ThirdPartyWeatherAPI $api;

    public function __construct(ThirdPartyWeatherAPI $api) {
        $this->api = $api;
    }

    public function getTemperature(string $city): float {
        return ($this->api->getTemperatureInFahrenheit($city) - 32) * 5 / 9;
    }
}

$weather = new WeatherAdapter(new ThirdPartyWeatherAPI());
echo $weather->getTemperature("New York") . PHP_EOL;
```

---

# **3. Bad Implementations of the Adapter Pattern**
Here’s how you can **recognize bad implementations** of the Adapter Pattern:

1. **Modifying the existing class instead of using an Adapter**
   - If you modify the existing system directly to work with the new interface, you're **violating the Open/Closed Principle**.

2. **Creating an Adapter that only duplicates functionality**
   - If your adapter does nothing but **call the original method** without any transformation, then it’s redundant.

3. **Making an Adapter that is too tightly coupled**
   - Your adapter should only depend on the interface, not on a **specific concrete implementation**.

---

# **4. Summary**
- The **Adapter Pattern** allows incompatible interfaces to work together.
- It is widely used in **third-party API integrations, legacy system migration, and cross-platform development**.
- A **good adapter should not modify existing code but instead, act as a middle layer**.
- Avoid **bad implementations** that modify the original system or introduce unnecessary complexity.
