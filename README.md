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
