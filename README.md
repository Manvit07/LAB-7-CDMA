# LAB-7-CDMA

clc;
clear;
close all;
% Parameters
N = 7; % Length of the spreading code (chip length)
c = [0 0 1 0 1 1 1]; % Spreading code (chip sequence)
b = randi([0 1], 1, 2); % Random binary data to transmit
S = 700; % Samples per bit for NRZ pulse shaping
% Binary to polar mapping (BPSK): map 0 to -1 and 1 to +1
cm = zeros(1, length(c));
for i = 1:length(c)
if c(i) == 0
cm(i) = -1; % Map 0 to -1
else
cm(i) = 1; % Map 1 to +1
end
end
% Information-bearing signal (b)
m = []; % To store the mapped signal
% Loop through the bits in 'b' and map them using the spreading code
for k = 1:length(b)
if b(k) == 0
mm = -cm; % For bit 0, map to the negative of the spreading code
else
mm = cm; % For bit 1, map to the spreading code
end

m = [m mm]; % Append the mapped sequence to 'm'
end
% NRZ Pulse Shaping: Time vector should span the total signal duration
t = (0:1/S:(length(b) * N * S) - 1); % Time vector (length of the signal in samples)
y = zeros(1, length(t)); % Initialize the output signal
i = 1; % Initialize the chip index
chip_duration = 1 / N; % Duration of each chip
for j = 1:length(t)
if i > length(m)
break;
end
if mod(j-1, S) == 0 && i <= length(m)
y(j) = m(i); % Assign the value of the chip to the signal
i = i + 1; % Move to the next chip
else
y(j) = m(i); % Keep the current chip value for the remainder of the chip duration
end
end
% Plot DSSS baseband signal
figure;
subplot(3, 1, 1);
plot(t, y);
axis tight;
xlabel('Time');
ylabel('Amplitude');
title('DSSS Baseband Signal');
% Carrier signal (a simple cosine)
f_c = 10;
carrier = cos(2 * pi * f_c * t);
subplot(3, 1, 2);
plot(t, carrier);
axis tight;
xlabel('Time');
ylabel('Amplitude');
title('Sinusoidal Carrier Signal');
% DSSS BPSK signal (modulated signal)
x = y .* carrier;
subplot(3, 1, 3);
plot(t, x);
axis tight;
xlabel('Time');
ylabel('Amplitude');
title('DSSS BPSK Signal');
