# Chuck
#Project programming music in ChucK
//Sound Network
SinOsc bass => Pan2 p => dac;
SndBuf sfx => dac;
Mandolin m => Pan2 pm => dac;
SndBuf kick => dac;
SndBuf hihat => dac;
Shakers shak => dac;
Sitar sitar => dac;
ModalBar mod => dac;

//set volumes
.3 =>kick.gain;
.5 => hihat.gain;
0=>bass.gain;
0=>sfx.gain;

//Db Phrygian array
[49, 50, 52, 54, 56, 57, 59, 61] @=> int Db[];

//load files into sndbuf
me.dir() + "/audio/kick_01.wav" => kick.read;
me.dir() + "/audio/hihat_01.wav" => hihat.read;


//time parameters
.75 => float beat; 
.50 => float trip;
beat/2.0 => float eight;
eight/2.0 => float sixt;
sixt/2.0 => float thirty2;

//rhythm arrays
[1,1,1,0,0,0,0,0] @=> int kick_ptrn_1[];
[1,0,1,1,0,0,0,1] @=> int hihat_ptrn_1[];
[1,1,1,0,0,1,1,0] @=> int kick_ptrn_2[];
[1,0,1,1,1,0,0,1] @=> int hihat_ptrn_2[];
// Bell rhythms
[eight, sixt, eight, sixt, eight, sixt, eight, sixt, eight, sixt] @=> float bellrhy[];

//beat function
fun void groove( int kickArray[], int hihatArray[], float beattime )
{
    for( 0 => int b; b < kickArray.cap(); b++ )
    {
        if( kickArray[b] == 1 )
        {
            0 => kick.pos;
        }
        if( hihatArray[b] == 1 )
        {
            0 => hihat.pos;
        }
        beattime::second => now;
    }
}


//MAIN PROGRAM
for ( 0 => int Call; Call < 2; Call++ )
{
    //play mandolin randomly from Db Phrygian Array
    for ( 0 => int A; A < Db.cap(); A++ )
    {
        Std.mtof(Db[Math.random2(0,Db.cap()-1)])*2 => m.freq;
        Math.random2f(0.1,0.7) => m.noteOn;
        sixt::second => now;
    }
    
    //call drum groove from array
    groove( kick_ptrn_1, hihat_ptrn_1, sixt);
    
    //play sitar randomly from Db Phrygian Array
    for ( 0 => int A; A < Db.cap(); A++ )
    {
        Std.mtof(Db[Math.random2(0,Db.cap()-1)])*2 => sitar.freq;
        Math.random2f(0.1,0.7) => sitar.noteOn;
        sixt::second => now;
        
    }
//turn off mandolin and sitar
1 => m.noteOff;
beat::second => now;
1 => sitar.noteOff;
beat::second => now;    
}

//Bell sounds
for ( 0 => int Bing; Bing < 16; Bing++ )
{
    4 => mod.preset;
    Std.mtof(Db[Math.random2(0,Db.cap()-1)])*2 => mod.freq;
    1.0 => mod.noteOn;
    sixt::second => now;
}
for ( 0 => int Bing; Bing < 16; Bing++ )
{
    4 => mod.preset;
    Std.mtof(Db[Math.random2(0,Db.cap()-1)])*2 => mod.freq;
    1.0 => mod.noteOn;
    thirty2::second => now;
}


for ( 0 => int B; B < 2; B++ )
{
    //Mortal Kombat Mandolin
    Std.mtof(Db[B])/4 => m.freq;
    0.1 => m.bodySize;
    1.0 => m.noteOn;
    //call drum groove from array
    groove( kick_ptrn_2, hihat_ptrn_2, sixt);
}
    
for ( 0 => int B; B < 4; B++ )
{
    //Mortal Kombat Mandolin
    Math.sin( now / 1::second *2*pi ) => pm.pan;
    1::ms => now;
    Std.mtof(Db[B])/4 => m.freq;
    0.1 => m.bodySize;
    1.0 => m.noteOn;
    //call drum groove from array
    groove( kick_ptrn_2, hihat_ptrn_2, sixt);
}

for ( 0 => int B; B < 1; B++ )
{
    //Mortal Kombat Mandolin
    Std.mtof(Db[B])/4 => m.freq;
    0.1 => m.bodySize;
    1.0 => m.noteOn;
    //call drum groove from array
    groove( kick_ptrn_2, hihat_ptrn_2, sixt);
}
//New Mandolin
SinOsc s => Mandolin mo => dac;

3.0 => s.freq;
10.0 => s.gain;
1 => mo.gain;

34.65 => mo.freq;
1.0 => mo.noteOn;
3::second => now;


