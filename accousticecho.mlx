function AcousticEchoCancellationGUI()
    % Create the main GUI window
    fig = uifigure('Position', [100, 100, 800, 600], 'Name', 'Acoustic Echo Cancellation');

    % Create buttons for actions
    loadButton = uibutton(fig, 'Position', [20, 550, 120, 40], 'Text', 'Load Audio', ...
        'ButtonPushedFcn', @(btn, event) loadAudioCallback());
    cancelButton = uibutton(fig, 'Position', [160, 550, 120, 40], 'Text', 'Cancel Echo', ...
        'ButtonPushedFcn', @(btn, event) cancelEchoCallback(), 'Enable', 'off');
    playOriginalButton = uibutton(fig, 'Position', [300, 550, 120, 40], 'Text', 'Play Original', ...
        'ButtonPushedFcn', @(btn, event) playOriginalCallback(), 'Enable', 'off');
    playOutputButton = uibutton(fig, 'Position', [440, 550, 120, 40], 'Text', 'Play Output', ...
        'ButtonPushedFcn', @(btn, event) playOutputCallback(), 'Enable', 'off');

    % Create plots for audio signals
    ax1 = axes(fig, 'Position', [0.1, 0.6, 0.35, 0.25]);
    title(ax1, 'Original Signal with Echo (Time Domain)');
    ax2 = axes(fig, 'Position', [0.55, 0.6, 0.35, 0.25]);
    title(ax2, 'Original Signal with Echo (Frequency Domain)');
    ax3 = axes(fig, 'Position', [0.1, 0.3, 0.35, 0.25]);
    title(ax3, 'Output Signal after Echo Cancellation (Time Domain)');
    ax4 = axes(fig, 'Position', [0.55, 0.3, 0.35, 0.25]);
    title(ax4, 'Output Signal after Echo Cancellation (Frequency Domain)');

    % Global variables for audio
    inputSignal = [];
    fs = [];
    outputSignal = [];

    % Load audio callback
    function loadAudioCallback()
        [file, path] = uigetfile('*.mp3;*.wav', 'Select Audio File');
        if isequal(file, 0)
            return;
        end
        [inputSignal, fs] = audioread(fullfile(path, file));
        inputSignal = inputSignal(:, 1); % Use the first channel if stereo
        inputSignal = inputSignal / max(abs(inputSignal)); % Normalize

        % Compute frequency domain spectrum of original signal
        L = length(inputSignal);
        spectrum = fftshift(fft(inputSignal)); % Shift zero frequency to the center
        freq = linspace(-fs/2, fs/2, L); % Frequency axis

        % Enable buttons and plot the original signal
        cancelButton.Enable = 'on';
        playOriginalButton.Enable = 'on';
        plot(ax1, (1:L) / fs, inputSignal);
        xlabel(ax1, 'Time (s)');
        ylabel(ax1, 'Amplitude');
        title(ax1, 'Original Signal with Echo (Time Domain)');

        % Plot the frequency domain spectrum as sticks
        stem(ax2, freq, abs(spectrum), 'b', 'Marker', 'o');
        xlabel(ax2, 'Frequency (Hz)');
        ylabel(ax2, 'Magnitude');
        title(ax2, 'Original Signal with Echo (Frequency Domain)');
        axis(ax2, [-fs/2 fs/2 0 max(abs(spectrum))+500]);
    end

    % Echo cancellation callback 
    function cancelEchoCallback()
        % Parameters for the adaptive filter
        N = 256; % Number of filter taps
        M = 128; % Block size
        mu = 0.01; % Step size for the adaptive filter
        numBlocks = ceil(length(inputSignal) / M); % Adjust number of blocks for last incomplete block

        % Initialize variables
        W = 0.01 * randn(N, 1); % Initialize filter coefficients with small random values
        outputSignal = zeros(size(inputSignal)); % Output signal
        errorSignal = zeros(size(inputSignal)); % Error signal

        % Adaptive filtering in frequency domain
        for k = 1:numBlocks
            % Get the current block
            blockStart = (k-1)*M + 1;
            blockEnd = min(k*M, length(inputSignal)); % Ensure last block doesn't go out of bounds
            block = inputSignal(blockStart:blockEnd);

            % Zero-pad the block to match the filter size (N)
            paddedBlock = [block(:); zeros(N - length(block), 1)]; % Ensure both block and padding are column vectors

            % Perform FFT on the block
            X = fft(paddedBlock, N); % Input block in frequency domain

            % Calculate the output of the adaptive filter
            Y = X .* W; % Filtered output in frequency domain

            % Calculate the error signal
            E = X - Y; % Error in frequency domain
            e_time = real(ifft(E)); % Convert error back to time domain
            errorSignal(blockStart:blockEnd) = e_time(1:length(block)); % Match length to the block size

            % Update the filter coefficients
            W = W + mu * conj(E) .* X ./ (abs(X).^2 + 1e-6); % Avoid division by zero
        end

        % Normalize the output signal
        outputSignal = errorSignal / max(abs(errorSignal));

        % Compute frequency domain spectrum of output signal
        L = length(outputSignal);
        spectrum = fftshift(fft(outputSignal)); % Shift zero frequency to the center
        freq = linspace(-fs/2, fs/2, L); % Frequency axis

        % Plot the output signal
        plot(ax3, (1:L) / fs, outputSignal, 'r');
        xlabel(ax3, 'Time (s)');
        ylabel(ax3, 'Amplitude');
        title(ax3, 'Output Signal after Echo Cancellation (Time Domain)');

        % Plot the frequency domain spectrum as sticks
        stem(ax4, freq, abs(spectrum), 'r', 'Marker', 'o');
        xlabel(ax4, 'Frequency (Hz)');
        ylabel(ax4, 'Magnitude');
        title(ax4, 'Output Signal after Echo Cancellation (Frequency Domain)');
        axis(ax4, [-fs/2 fs/2 0 max(abs(spectrum))+500]);

        % Enable the play output button
        playOutputButton.Enable = 'on';
    end

    % Play the original signal
    function playOriginalCallback()
        if ~isempty(inputSignal)
            sound(inputSignal, fs);
        end
    end

    % Play the output signal
    function playOutputCallback()
        if ~isempty(outputSignal)
            sound(outputSignal, fs);
        end
    end
end
