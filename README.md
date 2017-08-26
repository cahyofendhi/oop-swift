# oop-swift
learning oop in swift

//: Playground - noun: a place where people can play

class Instrument {
    
    let brand: String
    
    init(brand: String){
        
        self.brand	= brand
        
    }
    
    func tune() -> String {
        fatalError("Implement this method for\(self.brand)")
    }
    
    func play(_ music: Music) -> String {
        return music.prepared()
    }
    
    func perform(_ music: Music) {
        print(tune())
        print(play(music))
    }
    
}

class Music {
    
    let notes: [String]
    
    init(notes: [String]){
        self.notes	= notes
    }
    
    func prepared() -> String {
        return notes.joined(separator: " ")
    }
    
}

class Piano: Instrument {
    
    var hasPedals	= Bool()
    
    static let whiteKeys	= 52
    static let blackKeys	= 36
    
    init(brand: String, hasPedals: Bool = false){
        self.hasPedals	= hasPedals
        super.init(brand: brand)
    }
    
    override func tune() -> String {
        return "Piano standart tuning for \(brand)."
    }
    
    override func play(_ music: Music) -> String {
        return play(music, usingPedals: hasPedals)
    }
    
    func play(_ music: Music, usingPedals: Bool) -> String {
        let preparedNotes = super.play(music)
        if hasPedals && usingPedals {
            return "Play piano notes \(preparedNotes) with pedals."
        }
        else {
            return "Play piano piano notes \(preparedNotes) without pedals"
        }
    }
}

class Guitar: Instrument {

    var stringGuide = String()
    
    init(brand: String, stringGuide: String = "medium") {
        self.stringGuide    = stringGuide
        super.init(brand: brand)
    }
}

class AccousticGuitar: Guitar {

    static let numberOfStrings = 6
    static let fretCount = 20
    
    override func tune() -> String {
        return "Tune \(brand) accoustic with C D E F G A B C"
    }
    
    override func play(_ music: Music) -> String {
        let preparedNoted = super.play(music)
        return "Play folk tune on frets \(preparedNoted)."
        
    }
    
}

class Amplifier {

    private var _volume: Int
    private(set) var isOn: Bool
    
    init() {
        isOn    = false
        _volume = 0
    }
    
    func plugIn() {
        isOn    = true
    }
    
    func unPlug() {
        isOn    = false
    }
    
    var volume: Int {
    
        get {
            return isOn ? _volume : 0
        }
        
        set {
            _volume = min(max(newValue, 0), 10)
        }
    
    }
    
}

class ElectricGuitar: Guitar {
    
    var amplifier = Amplifier()
    
    init(brand: String, stringGauge: String = "light", amplifier: Amplifier) {
        self.amplifier  = amplifier
        super.init(brand: brand, stringGuide: stringGauge)
    }

    override func tune() -> String {
        amplifier.plugIn()
        amplifier.volume    = 5
        return "Tune \(brand) electric with E A D G B E"
    }
    
    override func play(_ music: Music) -> String {
        let preparedNotes = super.play(music)
        return "Play solo \(preparedNotes) at volume \(amplifier.volume)"
        
    }
    

}

class BassGuitar: Guitar {
    
    let amplifier = Amplifier()
    
    init(brand: String, stringGauge: String = "heavy", amplifier: Amplifier) {
        super.init(brand: brand, stringGuide: stringGauge)
    }
    
    override func tune() -> String {
        amplifier.plugIn()
        return "Tune \(brand) bass with E A D G"
    }

    override func play(_ music: Music) -> String {
        let preparedNotes = super.play(music)
        return "Play bass line \(preparedNotes) at volume \(amplifier.volume)."
        
    }

}

class Band {

    let instruments: [Instrument]
    
    init(instruments: [Instrument]) {
        self.instruments    = instruments
    }

    func perform(_ music: Music){
        for instrument in instruments {
            instrument.perform(music)
        }
    }
    
}

let piano   = Piano(brand: "Yamaha", hasPedals: true)
piano.tune()
let music = Music(notes: ["C", "G", "F"])
piano.play(music, usingPedals: false)
piano.play(music)

Piano.whiteKeys
Piano.blackKeys

let accousticGuitar = AccousticGuitar(brand: "Honda", stringGuide: "mark")
let amplifier       = Amplifier()
let electricGuitar  = ElectricGuitar(brand: "Suzukki", stringGauge: "Ianone", amplifier: amplifier)
let bassGuitar      = BassGuitar(brand: "KTM", stringGauge: "Jack Milner", amplifier: amplifier)

let instruments     = [piano, accousticGuitar, electricGuitar, bassGuitar]
let band            = Band(instruments: instruments)
band.perform(music)





