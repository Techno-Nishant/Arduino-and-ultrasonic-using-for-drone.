# Arduino-and-ultrasonic-using-for-drone.
[span_0](start_span)#include <NewPing.h> // Download this library by going to <Tools> & then <Manage Libraries>[span_0](end_span)
[span_1](start_span)#include <Servo.h>   // Download this library by going to <Tools> & then <Manage Libraries>[span_1](end_span)
[span_2](start_span)#include <FastLED.h> // Download this library from github[span_2](end_span)

[span_3](start_span)CRGB leds[1];[span_3](end_span)

Servo out1; [span_4](start_span)//roll[span_4](end_span)
Servo out2; [span_5](start_span)//pitch[span_5](end_span)
Servo out3; [span_6](start_span)//throttle[span_6](end_span)

[span_7](start_span)int trig1 = 2;[span_7](end_span)
[span_8](start_span)int echo1 = 2;[span_8](end_span)
//int trig2 = A1; [span_9](start_span)// Commented out in source[span_9](end_span)
//int echo2 = A1; [span_10](start_span)// Commented out in source[span_10](end_span)
[span_11](start_span)int trig3 = 4;[span_11](end_span)
[span_12](start_span)int echo3 = 4;[span_12](end_span)
[span_13](start_span)int trig4 = 5;[span_13](end_span)
[span_14](start_span)int echo4 = 5;[span_14](end_span)
[span_15](start_span)int trig5 = 6;[span_15](end_span)
[span_16](start_span)int echo5 = 6;[span_16](end_span)
[span_17](start_span)int trig6 = 7;[span_17](end_span)
[span_18](start_span)int echo6 = 7;[span_18](end_span)
[span_19](start_span)int MAX_DISTANCE = 400;[span_19](end_span)

[span_20](start_span)NewPing sonar1(trig1, echo1, MAX_DISTANCE);[span_20](end_span)
//NewPing sonar2(trig2,echo2,MAX_DISTANCE); [span_21](start_span)// Commented out in source[span_21](end_span)
[span_22](start_span)NewPing sonar3(trig3, echo3, MAX_DISTANCE);[span_22](end_span)
[span_23](start_span)NewPing sonar4(tr4, echo4, MAX_DISTANCE);[span_23](end_span)
[span_24](start_span)NewPing sonar5(trig5, echo5, MAX_DISTANCE);[span_24](end_span)
[span_25](start_span)NewPing sonar6(trig6, echo6, MAX_DISTANCE);[span_25](end_span)

[span_26](start_span)float dist1, dist2, dist3, dist4, dist5, dist6;[span_26](end_span)
[span_27](start_span)unsigned long counter_1, counter_2, counter_3, current_count;[span_27](end_span)
[span_28](start_span)byte last_CH1_state, last_CH2_state, last_CH3_state;[span_28](end_span)
int input_PITCH;    [span_29](start_span)//input on D9 of arduino[span_29](end_span)
int input_ROLL;     [span_30](start_span)//input on D8 of arduino[span_30](end_span)
int input_THROTTLE; [span_31](start_span)//input on D10 of arduino[span_31](end_span)
[span_32](start_span)int DN;[span_32](end_span)
[span_33](start_span)int UP;[span_33](end_span)
[span_34](start_span)int antiDN;[span_34](end_span)
[span_35](start_span)int prev_time = 0;[span_35](end_span)

void setup() {
    // Serial.begin(115200); [span_36](start_span)// Commented out in source[span_36](end_span)
    [span_37](start_span)FastLED.addLeds<WS2812, 3, GRB>(leds, 1);[span_37](end_span)
    [span_38](start_span)PCICR |= (1 << PCIE0);[span_38](end_span)
    [span_39](start_span)PCMSK0 |= (1 << PCINT0);[span_39](end_span)
    [span_40](start_span)PCMSK0 |= (1 << PCINT1);[span_40](end_span)
    [span_41](start_span)PCMSK0 |= (1 << PCINT2);[span_41](end_span)
    out1.attach(13); [span_42](start_span)//roll[span_42](end_span)
    out2.attach(11); [span_43](start_span)//pitch[span_43](end_span)
    out3.attach(12); [span_44](start_span)//throttle[span_44](end_span)
    [span_45](start_span)pinMode(A0, INPUT);[span_45](end_span)
}

void loop() {
    [span_46](start_span)int a = 160;[span_46](end_span)
    [span_47](start_span)int b = 160;[span_47](end_span)
    [span_48](start_span)int c = 160;[span_48](end_span)
    [span_49](start_span)int d = 160;[span_49](end_span)
    [span_50](start_span)int e = 160;[span_50](end_span)

    [span_51](start_span)if (input_ROLL > 1600) {[span_51](end_span)
        [span_52](start_span)d = 210;[span_52](end_span)
    [span_53](start_span)} else if (input_ROLL < 1400) {[span_53](end_span)
        [span_54](start_span)c = 210;[span_54](end_span)
    }

    [span_55](start_span)if (input_PITCH > 1600) {[span_55](end_span)
        [span_56](start_span)a = 210;[span_56](end_span)
    [span_57](start_span)} else if (input_PITCH < 1400) {[span_57](end_span)
        [span_58](start_span)b = 210;[span_58](end_span)
    }

    [span_59](start_span)dist1 = sonar1.ping_cm();[span_59](end_span)
    [span_60](start_span)dist2 = sonar6.ping_cm();[span_60](end_span)
    [span_61](start_span)dist3 = sonar3.ping_cm();[span_61](end_span)
    [span_62](start_span)dist4 = sonar4.ping_cm();[span_62](end_span)
    [span_63](start_span)dist5 = sonar5.ping_cm();[span_63](end_span)
    dist6 = 200; // This Sensor is disabled due to my bad sensor. [span_64](start_span)If you want to enable just replace "200" with "sonar6.ping_cm()"[span_64](end_span)

    byte roll = map(input_ROLL, 1108, 1856, 55, 125);   [span_65](start_span)//mapping roll[span_65](end_span)
    byte pitch = map(input_PITCH, 1180, 1856, 55, 125); [span_66](start_span)//mapping pitch[span_66](end_span)
    byte throttle = map(input_THROTTLE, 1160, 1830, 55, 125); [span_67](start_span)//mapping throttle[span_67](end_span)

    byte antifront = map(dist1, 6, 210, 55, 60);  [span_68](start_span)//force to back[span_68](end_span)
    byte antiback = map(dist2, 6, 210, 125, 120); [span_69](start_span)//force to front[span_69](end_span)
    byte antileft = map(dist3, 6, 210, 125, 120); [span_70](start_span)//force to right[span_70](end_span)
    byte antiright = map(dist4, 6, 210, 55, 60);  [span_71](start_span)//force to left[span_71](end_span)
    byte antidown = map(dist6, 1, 100, 125, 120); [span_72](start_span)//force to up[span_72](end_span)
    byte antiUP = map(dist5, 2, 210, 55, 60);     [span_73](start_span)//force to down[span_73](end_span)
    [span_74](start_span)int val = pulseIn(A0, HIGH);[span_74](end_span)

    [span_75](start_span)if (val < 1500) {[span_75](end_span)
        // For Pitch..............
        [span_76](start_span)if (dist1 < a && dist1 > 0 && (dist2 > b || dist2 == 0)) {[span_76](end_span)
            [span_77](start_span)out2.write(antifront);[span_77](end_span)
            [span_78](start_span)b = 210;[span_78](end_span)
        [span_79](start_span)} else if ((dist1 >= a || dist1 == 0) && (dist2 >= b || dist2 == 0)) {[span_79](end_span)
            [span_80](start_span)out2.write(pitch);[span_80](end_span)
        [span_81](start_span)} else if (dist2 < b && dist2 > 0 && (dist1 > a || dist1 == 0)) {[span_81](end_span)
            [span_82](start_span)out2.write(antiback);[span_82](end_span)
            [span_83](start_span)a = 210;[span_83](end_span)
        [span_84](start_span)} else if (dist1 < a && dist2 < b && dist1 > 0 && dist2 > 0) {[span_84](end_span)
            [span_85](start_span)out2.write(92);[span_85](end_span)
            [span_86](start_span)c = 210;[span_86](end_span)
            [span_87](start_span)d = 210;[span_87](end_span)
            [span_88](start_span)e = 210;[span_88](end_span)
            [span_89](start_span)if (dist3 > c || dist3 == 0) {[span_89](end_span)
                [span_90](start_span)out1.write(55);[span_90](end_span)
            [span_91](start_span)} else if (dist4 > d || dist4 == 0) {[span_91](end_span)
                [span_92](start_span)out1.write(125);[span_92](end_span)
            [span_93](start_span)} else if (dist5 > e || dist5 == 0) {[span_93](end_span)
                [span_94](start_span)out3.write(110);[span_94](end_span)
            [span_95](start_span)} else if (dist6 > 50 || dist6 == 0) {[span_95](end_span)
                [span_96](start_span)out3.write(60);[span_96](end_span)
            }
        }

        // For Roll..............
        [span_97](start_span)if (dist3 < c && dist3 > 0 && (dist4 > d || dist4 == 0)) {[span_97](end_span)
            [span_98](start_span)out1.write(antileft);[span_98](end_span)
            [span_99](start_span)d = 210;[span_99](end_span)
        [span_100](start_span)} else if (dist3 >= c || dist3 == 0 && dist4 >= d || dist4 == 0) {[span_100](end_span)
            [span_101](start_span)out1.write(roll);[span_101](end_span)
        [span_102](start_span)} else if (dist4 < d && dist4 > 0 && (dist3 > c || dist3 == 0)) {[span_102](end_span)
            [span_103](start_span)out1.write(antiright);[span_103](end_span)
            [span_104](start_span)c = 210;[span_104](end_span)
        [span_105](start_span)} else if (dist3 < c && dist4 < d && dist3 > 0 && dist4 > 0) {[span_105](end_span)
            [span_106](start_span)out1.write(92);[span_106](end_span)
            [span_107](start_span)a = 210;[span_107](end_span)
            [span_108](start_span)b = 210;[span_108](end_span)
            [span_109](start_span)e = 210;[span_109](end_span)
            [span_110](start_span)if (dist1 > a || dist1 == 0) {[span_110](end_span)
                [span_111](start_span)out2.write(125);[span_111](end_span)
            [span_112](start_span)} else if (dist2 > b || dist2 == 0) {[span_112](end_span)
                [span_113](start_span)out2.write(55);[span_113](end_span)
            [span_114](start_span)} else if (dist5 > e || dist5 == 0) {[span_114](end_span)
                [span_115](start_span)out3.write(110);[span_115](end_span)
            [span_116](start_span)} else if (dist6 > 50 || dist6 == 0) {[span_116](end_span)
                [span_117](start_span)out3.write(60);[span_117](end_span)
            }
        }

        // For Throttle..............
        [span_118](start_span)if (dist5 < e && dist5 > 0 && (dist6 > 50 || dist6 == 0)) {[span_118](end_span)
            [span_119](start_span)out3.write(antiUP);[span_119](end_span)
        [span_120](start_span)} else if ((dist5 >= e || dist5 == 0) && (dist6 >= 50 || dist6 == 0)) {[span_120](end_span)
            [span_121](start_span)out3.write(throttle);[span_121](end_span)
        [span_122](start_span)} else if (dist6 < 50 && dist6 > 0 && (dist5 > e || dist5 == 0)) {[span_122](end_span)
            [span_123](start_span)out3.write(antidown);[span_123](end_span)
        [span_124](start_span)} else if (dist5 < e && dist6 < 50 && dist5 > 0 && dist6 > 0) {[span_124](end_span)
            [span_125](start_span)out3.write(88);[span_125](end_span)
            [span_126](start_span)a = 210;[span_126](end_span)
            [span_127](start_span)b = 210;[span_127](end_span)
            [span_128](start_span)c = 210;[span_128](end_span)
            [span_129](start_span)d = 210;[span_129](end_span)
            [span_130](start_span)if (dist1 > a || dist1 == 0) {[span_130](end_span)
                [span_131](start_span)out2.write(125);[span_131](end_span)
            [span_132](start_span)} else if (dist2 > b || dist2 == 0) {[span_132](end_span)
                [span_133](start_span)out2.write(55);[span_133](end_span)
            [span_134](start_span)} else if (dist3 > c || dist3 == 0) {[span_134](end_span)
                [span_135](start_span)out1.write(125);[span_135](end_span)
            [span_136](start_span)} else if (dist4 > d || dist4 == 0) {[span_136](end_span)
                [span_137](start_span)out1.write(55);[span_137](end_span)
            }
        }

        [span_138](start_span)if (input_THROTTLE <= 1400) {[span_138](end_span)
            [span_139](start_span)out3.write(throttle);[span_139](end_span)
        }

        [span_140](start_span)int FB = (dist1 < a && dist2 < b && dist1 > 0 && dist2 > 0);[span_140](end_span)
        [span_141](start_span)int LR = (dist3 < c && dist4 < d && dist3 > 0 && dist4 > 0);[span_141](end_span)

        [span_142](start_span)if (FB && LR) {[span_142](end_span)
            [span_143](start_span)out1.write(92);[span_143](end_span)
            [span_144](start_span)out2.write(92);[span_144](end_span)
            [span_145](start_span)if (input_THROTTLE <= 1400) {[span_145](end_span)
                [span_146](start_span)out3.write(throttle);[span_146](end_span)
            [span_147](start_span)} else if (dist5 > e || dist5 == 0) {[span_147](end_span)
                [span_148](start_span)out3.write(110);[span_148](end_span)
            [span_149](start_span)} else if (dist6 > 50) {[span_149](end_span)
                [span_150](start_span)out3.write(60);[span_150](end_span)
            } else {
                [span_151](start_span)out3.write(88);[span_151](end_span)
            }
        }
    [span_152](start_span)} else if (val >= 1500) {[span_152](end_span)
        [span_153](start_span)out1.write(roll);[span_153](end_span)
        [span_154](start_span)out2.write(pitch);[span_154](end_span)
        [span_155](start_span)out3.write(throttle);[span_155](end_span)
    }

    int f_clear = (dist1 > a || dist1 == 0);  [span_156](start_span)//front no obstacle[span_156](end_span)
    int b_clear = (dist2 > b || dist2 == 2);   [span_157](start_span)//back no obstacle[span_157](end_span)
    int l_clear = (dist3 > c || dist3 == 0);   [span_158](start_span)//left no obstacle[span_158](end_span)
    int r_clear = (dist4 > d || dist4 == 0);  [span_159](start_span)//right no obstacle[span_159](end_span)
    int up_clear = (dist5 > e || dist5 == 0);    [span_160](start_span)//up no obstacle[span_160](end_span)
    int down_clear = (dist6 > 50 || dist6 == 0); [span_161](start_span)//down no obstacle[span_161](end_span)

    [span_162](start_span)if (f_clear && b_clear && l_clear && r_clear && up_clear && down_clear) {[span_162](end_span)
        [span_163](start_span)if (val < 1500) {[span_163](end_span)
            [span_164](start_span)leds[0] = CRGB(0, 255, 0);[span_164](end_span)
            [span_165](start_span)FastLED.show();[span_165](end_span)
            [span_166](start_span)out1.write(roll);[span_166](end_span)
            [span_167](start_span)out2.write(pitch);[span_167](end_span)
            [span_168](start_span)out3.write(throttle);[span_168](end_span)
        [span_169](start_span)} else if (val >= 1500) {[span_169](end_span)
            [span_170](start_span)leds[0] = CRGB(235, 255, 200);[span_170](end_span)
            [span_171](start_span)FastLED.show();[span_171](end_span)
        }
    [span_172](start_span)} else if ((dist1 < a && dist1 > 0) || (dist2 < b && dist2 > 0) || (dist3 < c && dist3 > 0) || (dist4 < d && dist4 > 0) || (dist5 < e && dist5 > 0) || (dist6 < 50 && dist6 > 0)) {[span_172](end_span)
        [span_173](start_span)if (val < 1500) {[span_173](end_span)
            [span_174](start_span)leds[0] = CRGB(255, 0, 0);[span_174](end_span)
            [span_175](start_span)FastLED.show();[span_175](end_span)
        [span_176](start_span)} else if (val >= 1500) {[span_176](end_span)
            [span_177](start_span)leds[0] = CRGB(235, 255, 200);[span_177](end_span)
            [span_178](start_span)FastLED.show();[span_178](end_span)
        }
    }
[span_179](start_span)} //Here the main loop ends[span_179](end_span)


// The Below Codes Reads the PWM value of Receiver
[span_180](start_span)ISR(PCINT0_vect) {[span_180](end_span)
    [span_181](start_span)current_count = micros();[span_181](end_span)
    [span_182](start_span)if (PINB & B00000001) {[span_182](end_span)
        [span_183](start_span)if (last_CH1_state == 0) {[span_183](end_span)
            [span_184](start_span)last_CH1_state = 1;[span_184](end_span)
            [span_185](start_span)counter_1 = current_count;[span_185](end_span)
        }
    [span_186](start_span)} else if (last_CH1_state == 1) {[span_186](end_span)
        [span_187](start_span)last_CH1_state = 0;[span_187](end_span)
        [span_188](start_span)input_ROLL = current_count - counter_1;[span_188](end_span)
    }
    [span_189](start_span)if (PINB & B00000010) {[span_189](end_span)
        [span_190](start_span)if (last_CH2_state == 0) {[span_190](end_span)
            [span_191](start_span)last_CH2_state = 1;[span_191](end_span)
            [span_192](start_span)counter_2 = current_count;[span_192](end_span)
        }
    [span_193](start_span)} else if (last_CH2_state == 1) {[span_193](end_span)
        [span_194](start_span)last_CH2_state = 0;[span_194](end_span)
        [span_195](start_span)input_PITCH = current_count - counter_2;[span_195](end_span)
    }
    [span_196](start_span)if (PINB & B00000100) {[span_196](end_span)
        [span_197](start_span)if (last_CH3_state == 0) {[span_197](end_span)
            [span_198](start_span)last_CH3_state = 1;[span_198](end_span)
            [span_199](start_span)counter_3 = current_count;[span_199](end_span)
        }
    [span_200](start_span)} else if (last_CH3_state == 1) {[span_200](end_span)
        [span_201](start_span)last_CH3_state = 0;[span_201](end_span)
        [span_202](start_span)input_THROTTLE = current_count - counter_3;[span_202](end_span)
    }
}
