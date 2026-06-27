# Virtual_FPGA_EXPERIMENT
USe of TLV and Virtual FPGA LABS at MAKER's CHIP 


# Experiment First that want to Switch on an LED

```verilog
\m4_TLV_version 1d -p verilog --bestsv --noline: tl-x.org
\SV
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/Virtual-FPGA-Lab/main/tlv_lib/fpga_includes.tlv'])
   
\SV
   m5_lab()

\TLV
   /board
      /fpga
         |switch
            @0
               *slideswitch = 5'h3;
              
               *led[1] = *slideswitch[0] & *slideswitch[1];
      m4+board(/board, /fpga, 1, *, ['top: 0, left: -1500, width: 7000, height: 7000'])   // 3rd arg selects the zedboard. 
\SV
   endmodule
   
//\TLV
   
//   /board
      // Board selection:
      // 0 m5_FIRST_CLAAS_ID
      // 1 m5_ZEDBOARD_ID
      // 2 m5_ARTIX7_ID
      // 3 m5_BASYS3_ID
      // 4 m5_ICEBREAKER_ID
      // 5 m5_NEXYS_ID
      // 6 m5_CLEAR_ID
     // 3rd arg selects the board.
```  
# LED counter with switches to toggle the LED
```verilog
\m4_TLV_version 1d -p verilog --bestsv --noline: tl-x.org

\SV
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/Virtual-FPGA-Lab/main/tlv_lib/fpga_includes.tlv'])
   
\SV
   m5_lab()

\TLV
   /board
      /fpga
         |switch
            @0
               $count[7:0] <=  $count + 1;
               *slideswitch = 8'h81;
               
               *led[0] = $count[0];
               *led[6] = *slideswitch[0] & *slideswitch[7];
               *led[7] = *slideswitch[0] & *slideswitch[7];
      m4+board(/board, /fpga, 1, *, ['top: 0, left: -1500, width: 7000, height: 7000'])   // 3rd arg selects the board.
   
\SV
   endmodule
```

# LED COUNTING SEQUENCE ON ZEDBOARD
```verilog
\m4_TLV_version 1d -p verilog --bestsv --noline: tl-x.org
​
\SV
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/Virtual-FPGA-Lab/main/tlv_lib/fpga_includes.tlv'])
   
\SV
   m5_lab()
​
// Example using LEDs to display a binary counter.
\TLV example_fpga_logic(/_fpga)
   /board
      /fpga
         |led_pipe  // All TL-Verilog logic should be in a pipeline.
            @0   // Pipeline stage.
               // Run artificially slow in the real FPGA. 
               m4+fpga_heartbeat($refresh, 1, 50000000) 
               $reset = *reset;
               ?$refresh
                  $Leds[15:0] <= $reset ? 1 : $Leds + 1;
               // Drive $Leds to the board's LEDs.
               *led = $Leds;
               // Visualize the count driven to LEDs.
               \viz_js
                  box: {width: 100, height: 20, strokeWidth: 0},
                  render() {
                     return [new fabric.Text('$Leds'.asBinaryStr(), {left: 5, top: 5, fill: "yellow", fontSize: 10})]
                  }
​
\TLV
   /board
   
      // Board selection:
      // 0 - 1st CLaaS on AWS F1
      // 1 - Zedboard
      // 2 - Artix-7
      // 3 - Basys3
      // 4 - Icebreaker
      // 5 - Nexys
      // 6 - CLEAR
      m4+board(/board, /fpga, 1, *, , example_fpga_logic)   // 3rd arg selects the board.
​
\SV
   endmodule
​```
#BCD to Seven segment code
```python
\m4_TLV_version 1d -p verilog --bestsv --noline: tl-x.org
​
\SV
    m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/Virtual-FPGA-Lab/main/tlv_lib/fpga_includes.tlv'])
   
\SV
   m5_lab()
   
\TLV
   /board
      /fpga
         |seven_segment
            @0
               m4+fpga_heartbeat($refresh, 1, 500000)
               $reset = *reset;
               // 2-bit counter: counts 0,1,2,3 then wraps
               $count[3:0] = $reset ? 4'b0 : $refresh ? >>1$count + 1 : >>1$count;
               ?$refresh
                  //All 4 digits can be enabled by sending logic ‘0’.
                  //Each segment can be enabled by sending logic ‘0’
                  *sseg_digit_n = 4'b1101;                  
                  //*sseg_segment_n = 7'b1111000;
                  *sseg_segment_n =
                     ($count == 4'd0) ? 7'b1000000 :
                     ($count == 4'd1) ? 7'b1111001 :
                     ($count == 4'd2) ? 7'b0100100 :
                     ($count == 4'd3) ? 7'b0110000 :
                     ($count == 4'd4) ? 7'b0011001 :
                     ($count == 4'd5) ? 7'b0010010 :
                     ($count == 4'd6) ? 7'b0000010 :
                     ($count == 4'd7) ? 7'b1111000 :
                     ($count == 4'd8) ? 7'b0000000 :
                                        7'b0010000 ; // 9
                                        
                  //*sseg_decimal_point_n = 1'b1;
      m4+board(/board, /fpga, 3, *, ['top: 0, left: -1500, width: 7000, height: 7000'])   // 3rd arg selects the board.
​
\SV
   endmodule
```
