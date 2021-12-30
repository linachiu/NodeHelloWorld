### HTML

```
<ngx-dropzone (change)="onSelectFile($event)">
    <ngx-dropzone-label>Drop it, baby!</ngx-dropzone-label>
    <ngx-dropzone-preview *ngFor="let f of files" [removable]="removable[0]" (removed)="onRemoveFile(f)" style="height: 50px !important;  max-width: 80% !important;">
        <ngx-dropzone-label><div class="circle">V</div>{{ f.name }} ({{ f.type }})</ngx-dropzone-label>
        
    </ngx-dropzone-preview>
</ngx-dropzone>


<div class="custom-dropzone banner-img" ngx-dropzone [accept]="'image/*'"
    (change)="onSelect($event)" matInput>
    <ngx-dropzone-label>
        Click or Drag a file to upload
    </ngx-dropzone-label>
    <ngx-dropzone-image-preview ngProjectAs="ngx-dropzone-preview"
        *ngFor="let f of files" [file]="f" [removable]="true" (removed)="onRemove(f)">
        <ngx-dropzone-label>{{ f.name }} ({{ f.type }})</ngx-dropzone-label>
    </ngx-dropzone-image-preview>
</div>
                                
```



### TS

```
 onSelectFile(event) {
    console.log(event);
    this.files.push(...event.addedFiles);

    const formData = new FormData();

    // for (var i = 0; i < this.files.length; i++) { 
    //   // formData.append("file[]", this.files[i]);
    //   formData.append("fileList", this.files[i]);
    // }

    formData.append("file", this.files[0]);

    console.log(this.files)

    this.http.post('http://node-test-middle-atadc.bo-ose-test.micron.com/fileUploadSingle', formData)
    // this.http.post('http://localhost:3001/fileUploadSingle', formData)
    .subscribe(res => {
       console.log(res);
       alert('Uploaded Successfully.');
    })
  }

  onRemoveFile(event) {
      console.log(event);
      this.files.splice(this.files.indexOf(event), 1);
      this.isUserAllowedMTGroup();
  }
```
