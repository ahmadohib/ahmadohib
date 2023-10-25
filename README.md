% Data yang dimasukan adalah .dat ke dalam MATLAB
% Parameter untuk dilakukan FFT adalah nilai CBA
x = load('CBA.dat'); % Pastikan format file .dat sesuai dengan struktur data CBA
t = load('Waktu.dat') % Pastikan format file .dat sesuai dengan struktur data Waktu
%Mengatur Parameter
L = length(x);
Fs = mean(diff(t));
f = (0:L-1)*Fs/L;
%Melihat Distribusi data pengukuran setiap menitnya
figure(1)
plot(t,x,'*') %Virsualisasi data mentah pengukuran
grid on, xlabel('waktu (menit)'),ylabel('CBA (mGal)')

%Melakukan Proses FFT
X=(fft(x)); %Processing pada bilangan kompleks saat melakukan FFT pada nilai CBA
X_norm = 1/L*X
fprintf('Hasilnya dari proses FFT : %d\n', X);
figure(2)
plot(X,'*')
title('Hasil Proses dari FFT')
%Spektrum amplitudo dan fase gelombang setiap waktunya
figure(3)
subplot(1,2,1)
stem(f,abs(X_norm),'x','linewidth',2),xlabel('frekuensi (Hz)'),ylabel('Amplitudo')
subplot(1,2,2)
stem(f,angle(X_norm),'x','linewidth',2),xlabel('frekuensi (Hz)'),ylabel('sudut fase')
% Menampilkan plot analisis spektral Bil Gel Vs LnP
magnitude_fft = abs(X);
N = length(x);
frekuensi = (0:(N-1)) / N;
log_amplitude = log(frekuensi);
figure(4)
plot(log_amplitude, magnitude_fft);
xlabel('LnA (Logarithm Frekuensi)');
ylabel('Bil Gel (k)');
title('Plot Hasil FFT Bil Gel vs LnA')

%Melakukan filter
%1. filter highpass menggunakan butterworth
order = 4; % Order filter
cutoff_freq1 = 0.1; % Frekuensi cutoff
[magnitude_fft, log_amplitude] = butter(order, cutoff_freq1, 'high');
filtered_data = filter(magnitude_fft, log_amplitude,x);
% Plot hasil filter highpass
figure(5);
subplot(2,1,1); % Membuat subplot untuk membandingkan data asli dan hasil filter
plot(x);
title('Data Bouguer Anomaly Asli');
ylabel('Nilai Anomali Bouguer');
subplot(2,1,2);
plot(filtered_data);
title('Hasil Filter Highpass');
ylabel('Nilai Anomali Bouguer');
xlabel('Indeks Data');
%2. filter lowpass menggunakan butterworth
cutoff_freq2 = 0.01; % Frekuensi cutoff
[magnitude_fft, log_amplitude] = butter(order, cutoff_freq2, 'high');
filtered_data2 = filter(magnitude_fft, log_amplitude,x);
% Plot hasil filter lowpass
figure(6);
subplot(2,1,1); % Membuat subplot untuk membandingkan data asli dan hasil filter
plot(x);
title('Data Bouguer Anomaly Asli');
ylabel('Nilai Anomali Bouguer');
subplot(2,1,2);
plot(filtered_data2);
title('Hasil Filter lowpass');
ylabel('Nilai Anomali Bouguer');
xlabel('Indeks Data');
%3. filter bandpass menggunakan butterworth
low_cutoff_freq = 0.01; % Frekuensi cutoff bawah
high_cutoff_freq = 0.1; % Frekuensi cutoff atas
[magnitude_fft, log_amplitude] = butter(order,[low_cutoff_freq,high_cutoff_freq]/(Fs/2), 'bandpass');
filtered_data3 = filter(magnitude_fft, log_amplitude,x);
% Plot hasil filter bandpass
figure(7);
subplot(2,1,1); % Membuat subplot untuk membandingkan data asli dan hasil filter
plot(x);
title('Data Bouguer Anomaly Asli');
ylabel('Nilai Anomali Bouguer');
subplot(2,1,2);
plot(filtered_data3);
title('Hasil Filter bandpass');
ylabel('Nilai Anomali Bouguer');
xlabel('Indeks Data');

%Melakukan Inverse FFT(IFFT)
%1. Highpass
hasil_ifft = ifft(filtered_data);
figure(8);
subplot(2,1,1); % Membuat subplot untuk membandingkan data asli dan hasil filter
plot(filtered_data);
title('filter menggunakan highpass');
ylabel('Nilai Anomali Bouguer');
subplot(2,1,2);
plot(hasil_ifft);
title('Hasil IFFT highpass');
ylabel('Nilai Anomali Bouguer');
xlabel('Indeks Data');
%2. Lowpass
hasil_ifft2 = ifft(filtered_data2);
figure(9);
subplot(2,1,1); % Membuat subplot untuk membandingkan data asli dan hasil filter
plot(filtered_data2);
title('filter menggunakan lowpass');
ylabel('Nilai Anomali Bouguer');
subplot(2,1,2);
plot(hasil_ifft2);
title('Hasil IFFT lowpass');
ylabel('Nilai Anomali Bouguer');
xlabel('Indeks Data');
%3. Bandpass
hasil_ifft3 = ifft(filtered_data3);
figure(10);
subplot(2,1,1); % Membuat subplot untuk membandingkan data asli dan hasil filter
plot(filtered_data3);
title('filter menggunakan bandpass');
ylabel('Nilai Anomali Bouguer');
subplot(2,1,2);
plot(hasil_ifft3);
title('Hasil IFFT bandpass');
ylabel('Nilai Anomali Bouguer');
xlabel('Indeks Data');
<!---
ahmadohib/ahmadohib is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
