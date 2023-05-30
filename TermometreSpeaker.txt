module TermometreSpeaker (
  input wire [62:0] termometre,
  output reg [5:0] binary,
  output reg speaker
);

  reg [5:0] binary1, binary2;
  integer k, m;

  always @ (termometre) begin
    binary1 = 0;
    for (k = 1; k <= 32; k = k + 1) begin
      if (termometre[k-1] == 1'b1)
        binary1 = k;
    end
  end

  always @ (termometre) begin
    binary2 = 0;
    for (m = 1; m <= 31; m = m + 1) begin
      if (termometre[m+31] == 1'b1)
        binary2 = m;
    end
  end

  always @ (binary1 or binary2) begin
    if (binary2 > 0)
      binary = binary2 + 6'b100000;
    else
      binary = binary1;
  end

  always @ (termometre) begin
    if (binary < 6'd10)
      speaker = 1'b1;  // Ses çıkışını etkinleştir
    else
      speaker = 1'b0;  // Ses çıkışını devre dışı bırak
  end

endmodule




/////////////////////////////////////////////////////////




module TermometreSpeaker (
  input wire [62:0] termometre,
  output reg [5:0] binary,
  output reg speaker
);

  reg [5:0] binary1, binary2; // Termometre okuması için ikili değerler
  integer k, m; // Döngü sayaçları

  always @ (termometre) begin
    binary1 = 0; // binary1'i başlangıç değeriyle başlat
    for (k = 1; k <= 32; k = k + 1) begin
      if (termometre[k-1] == 1'b1)
        binary1 = k; // Termometre okumasına bağlı olarak binary1'i güncelle
    end
  end

  always @ (termometre) begin
    binary2 = 0; // binary2'yi başlangıç değeriyle başlat
    for (m = 1; m <= 31; m = m + 1) begin
      if (termometre[m+31] == 1'b1)
        binary2 = m; // Termometre okumasına bağlı olarak binary2'yi güncelle
    end
  end

  always @ (binary1 or binary2) begin
    if (binary2 > 0)
      binary = binary2 + 6'b100000; // Eğer binary2 sıfırdan farklı ise, binary'yi binary2 + 32 olarak güncelle
    else
      binary = binary1; // Aksi halde binary'yi binary1'e eşitle
  end

  always @ (termometre) begin
    if (binary < 6'd10)
      speaker = 1'b1;  // Eğer binary 10'den küçükse, hoparlör çıkışını etkinleştir
    else
      speaker = 1'b0;  // Aksi halde hoparlör çıkışını devre dışı bırak
  end

endmodule
