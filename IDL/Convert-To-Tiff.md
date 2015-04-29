Convert To Tiff

通过IDL+Envi批量转换Envi标准文件为Tif格式。

#### Script

    pro converttif

      ;Workspace dir
      currentDir = "Y:\Thonatos.Yang\data\FlatField\"
      currentDes = "Y:\Thonatos.Yang\data\TiffStd\"
      
      ;File array to deal
      files=["n1-1","n1-2","n1-3","n1-7","n2-1","n2-11","n2-2","n2-3","n2-5","n2-8","n4-1","n4-10","n4-11","n4-12","n4-2","n4-3","n4-4","n4-5","n4-6","n4-7","n4-8","n4-9","n5-1","n5-10","n5-11","n5-12","n5-2","n5-3","n5-4","n5-5","n5-6","n5-7","n5-8","n5-9"]
      filesSize = 33

      ;working on 5
      for f=5,filesSize do begin 
            
          currentName = files[f]
          currentFileOrgn = currentDir+currentName
          currentFileDest = currentDes+currentName+".tif"
          
          print,"Compelte:" + string(f+1) + " in " + string(filesSize+1)
          print,"Deal File: "+currentFileOrgn
      
          ;获取待存储的文件fid
          envi_open_file,currentFileOrgn,r_fid = fid

          ;获取文件相关信息
          envi_file_query,fid,dims = dims,nb = nb

          ;调用函数输出为tiff文件
          ENVI_OUTPUT_TO_EXTERNAL_FORMAT, dims = dims,pos = lindgen(nb),out_name = currentFileDest,/tiff,fid = fid

          ;删除原文件
          envi_file_mng,id = fid,/remove
      
      endfor

      print,"All Completed."
      
    end

