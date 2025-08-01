# Verilog-projects
Day 5

Design
module usr #(
    parameter N = 8
)(in,out,mode,clk,rst);
   
    input in;
    output reg[N-1:0] out;
    input clk,rst;
    input mode;
    
    always@(posedge clk)
    begin
    if(rst)begin
    out<= {N{1'b0}};
    end
    else begin
    if(mode==0) begin //right shift
    out<={in,out[N-1:1]};
    end // right shift
    else if(mode==1) begin //left shift
    out<={out[N-2:0],in};
    end // left shift close
    end //reset else close
    end // always begin closed
endmodule

--------------------------------------------------------
Testbench

module usr_test();
    reg in;
    wire [7:0]out;
    reg clk,rst;
    reg mode;
    
    usr dut (.in(in),
              .out(out),
              .clk(clk),
              .rst(rst),
              .mode(mode));
      
    always #5 clk=~clk;
    initial begin
    $monitor("time=%0d,input=%b,rst=%b,mode=%b,out=%b",$time,in,rst,mode,out);
    clk=0;
    in=0;
    rst=1;
    mode=0;
    #10 rst=0;in=0;
    #10 in=1;
    #10 in=0;
    #10 in=1;
    #10 in=0;
    #10 in=1;
    #10 mode=1;in=1;
    #10 in=1;
    #10 in=0;
    #10 in=1;
    #10 in=0;
    #10 in=0;
    #10 in=1;
    #10 in=0;
    $finish;
    end
endmodule

