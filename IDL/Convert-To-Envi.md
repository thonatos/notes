Convert To Envi

通过IDL批量将移动平滑后的txt数据文件转换Envi标准文件。

#### Script

	pro port

        ;File array to deal
        files=["n1-1","n1-2","n1-3","n1-7","n2-1","n2-11","n2-2","n2-3","n2-5","n2-8","n4-1","n4-10","n4-11","n4-12","n4-2","n4-3","n4-4","n4-5","n4-6","n4-7","n4-8","n4-9","n5-1","n5-10","n5-11","n5-12","n5-2","n5-3","n5-4","n5-5","n5-6","n5-7","n5-8","n5-9"]

        currentDir = "X:\data\SmoothedText\"
        currentDes = "X:\data\SmoothedEnvi\"

        for f=0,33 do begin
          
            ;Define Vars    
            currentName = files[f]
            currentFileData = currentDir+currentName+".tif.smoothed.txt"
            currentFileInfo = currentDir+currentName+".tif.info.txt"
            currentFileDest = currentDes+currentName
            currentFileHead = currentDes+currentName+".hdr"
            print,"Deal File: "+currentFileData
            
            ;Read file info
            print,"Read File Info: "+currentFileInfo
            info=fltarr(3)
            openr,lun0,currentFileInfo,/get_lun
            readf,lun0,info
            free_lun,lun0
            print,"Show File Info:",info
            
            ;Assign value to x,y,z
            x=long(info[1])
            y=long(info[0])
            z=long(info[2])
            sum=long(x*y)
            print,"X,Y,Z,Sum:",x,y,z,sum
            
            ;Read input text
            data=fltarr(sum,520)
            openr,lun1,currentFileData,/get_lun
            readf,lun1,data
            free_lun,lun1
            
            ;Port file to envi
            band=fltarr(x,y,520)
            for i=0,519 do begin
              print,"Reform Band:",i
              band[*,*,i]=reform(data[*,i],x,y)
            endfor
            
            band_1=transpose(band,[1,0,2])
            openw,lun2,currentFileDest,/get_lun
            writeu,lun2,band_1
            free_lun,lun2
            
            ;Write *.hdr
            openw,lun3,currentFileHead,/get_lun
            printf,lun3,"ENVI"
            printf,lun3,"description = {"
            printf,lun3,"  File Imported into ENVI.}"
            printf,lun3,"samples = "+string(x)
            printf,lun3,"lines   = "+string(y)
            printf,lun3,"bands   = "+string(z)
            printf,lun3,"header offset = 0"
            printf,lun3,"file type = ENVI Standard"
            printf,lun3,"data type = 4"
            printf,lun3,"interleave = bsq"
            printf,lun3,"sensor type = Unknown"
            printf,lun3,"byte order = 0"
            printf,lun3,"wavelength units = Unknown"
            free_lun,lun3
            
            ;Clear Vars
            delvar,info,x,y,z,sum,data,band,band_1
            
        endfor
        
        print,"All Done."

    end
