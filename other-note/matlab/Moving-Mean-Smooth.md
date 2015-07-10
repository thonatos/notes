Moving Mean Smooth

使用Matlab通过移动平均对Tiff数据进行批量平滑处理。

#### Script

	% Clear Workspace Var
    clear;
    clc;

    % Read Tiff Dir & Save file name to
    tiffDir = 'Y:\Thonatos.Yang\data\TiffStd\';
    smoothedDir = 'Y:\Thonatos.Yang\data\Smoothed\';
    tiffFileArray = dir(tiffDir);

    for f = 6:length(tiffFileArray)
        
        currentTiffFile = strcat(tiffDir,tiffFileArray(f).name);
        currentSmoothed = strcat(smoothedDir,tiffFileArray(f).name);
        fprintf('Processing : %s\n',currentTiffFile);
        
        % Read Fiff file
        originalTiff = imread(currentTiffFile);
        
        % Size of Tiff array
        [Tx,Ty,Tz] = size(originalTiff);
        
        % Calc Piexls for each wavelength
        pixels = Tx*Ty;
        
        % Save Tiff file info to text;
        dlmwrite(strcat(currentSmoothed,'.info.txt'),[Tx,Ty,Tz],'delimiter', '\t');
        
        % Reshape Array
        for k = 1:520;
            fprintf('\tReshaping Band: %d\n',k);
            reshapedTiff(k,:) = reshape(originalTiff(:,:,k),1,pixels);
        end
        
        % Smooth data
        for i = 1:520  % number of wavelength
            fprintf('\tSmoothing Band : %d\n',i);
            for j = 1:size(reshapedTiff,2)  % number of sampling
                if i<=3
                    smoothedTiff(i,j) = mean(reshapedTiff(i:(i+2),j));
                elseif i>3 & i<=250
                    smoothedTiff(i,j) = mean(reshapedTiff((i-2):(i+2),j));
                elseif i>250 & i<=300
                    smoothedTiff(i,j) = reshapedTiff(i,j);
                elseif i>300 & i<=516
                    smoothedTiff(i,j) = mean(reshapedTiff((i-4):(i+4),j));
                else
                    smoothedTiff(i,j) = mean(reshapedTiff((i-4):i,j));
                end
            end
        end
        
       
        % Save Result to text
        dlmwrite(strcat(currentSmoothed,'.smoothed.txt'),smoothedTiff,'delimiter', '\t');
        
        % Calc Average Value Of Each Band ,Save Average Value to csv
        % csvwrite(strcat(currentSmoothed,'.average.csv'),mean(smoothedTiff,2)');
        
        % Show Proress
        fprintf('Processed %s : %s\n\n',strcat(num2str((f-2)/(length(tiffFileArray)-2)),'%'),currentTiffFile);
        
        % Cat Matrix
        %     if f<4
        %         sum = mean(smoothedTiff,2)';
        %     else
        %         tmp = sum;
        %         clear sum;
        %         sum = [tmp;mean(smoothedTiff,2)'];
        %     end
        
        % Clear Var
        clearvars -except tiffDir tiffFileArray smoothedDir f; %sum;
    end

    % Save Sum Value to csv
    % csvwrite(strcat(smoothedDir,'average.cat.csv'),sum);

    fprintf('All Processed');