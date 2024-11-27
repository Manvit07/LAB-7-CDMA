# LAB-7-CDMA

clc;
clear;
close all;

N = 7; 
c = [0 0 1 0 1 1 1];
b = randi([0 1], 1, 2); 
S = 700; 

cm = zeros(1, length(c));
for i = 1:length(c)
if c(i) == 0
cm(i) = -1; % Map 0 to -1
else
cm(i) = 1; % Map 1 to +1
end
end

m = []; 

for k = 1:length(b)
if b(k) == 0
mm = -cm; 
else
mm = cm; 
end

m = [m mm]; 
end

t = (0:1/S:(length(b) * N * S) - 1); 
y = zeros(1, length(t)); 
i = 1; 
chip_duration = 1 / N;
for j = 1:length(t)
if i > length(m)
break;
end
if mod(j-1, S) == 0 && i <= length(m)
y(j) = m(i); 
i = i + 1; 
else
y(j) = m(i); 
end
end

figure;
subplot(3, 1, 1);
plot(t, y);
axis tight;
xlabel('Time');
ylabel('Amplitude');
title('DSSS Baseband Signal');

f_c = 10;
carrier = cos(2 * pi * f_c * t);
subplot(3, 1, 2);
plot(t, carrier);
axis tight;
xlabel('Time');
ylabel('Amplitude');
title('Sinusoidal Carrier Signal');

x = y .* carrier;
subplot(3, 1, 3);
plot(t, x);
axis tight;
xlabel('Time');
ylabel('Amplitude');
title('DSSS BPSK Signal');
