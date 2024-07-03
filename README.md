module traffic_light_controller(
    input clk,
    input reset,
    output reg [1:0] light // 00 = Red, 01 = Green, 10 = Yellow
);

    // State encoding
    typedef enum logic [1:0] {
        RED = 2'b00,
        GREEN = 2'b01,
        YELLOW = 2'b10
    } state_t;

    state_t current_state, next_state;

    // State transition
    always_ff @(posedge clk or posedge reset) begin
        if (reset)
            current_state <= RED;
        else
            current_state <= next_state;
    end

    // Next state logic
    always_comb begin
        case (current_state)
            RED: next_state = GREEN;
            GREEN: next_state = YELLOW;
            YELLOW: next_state = RED;
            default: next_state = RED;
        endcase
    end

    // Output logic
    always_comb begin
        case (current_state)
            RED: light = 2'b00;
            GREEN: light = 2'b01;
            YELLOW: light = 2'b10;
            default: light = 2'b00;
        endcase
    end

endmodule
