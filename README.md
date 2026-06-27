# Virtual_FPGA_EXPERIMENT
USe of TLV and Virtual FPGA LABS at MAKER's CHIP 


# Experiment First that want to Switch on an LED
'''
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
      m4+board(/board, /fpga, 1, *, ['top: 0, left: -1500, width: 7000, height: 7000'])   // 3rd arg selects the board.
   
\SV
   endmodule
   '''
   
