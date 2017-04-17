<template>
    <div id="app">
        <form id="fileupload">
            <span class="btn btn-success fileinput-button">
                <i class="glyphicon glyphicon-plus"></i>
                <span>Add Files...</span>
                <input type="file" name="files[]" multiple="">
            </span>
            <table role="presentation" class="table table-striped"><tbody class="files"></tbody></table>
        </form>
    </div>
</template>

<script>
import $ from 'jquery'
import 'blueimp-file-upload/js/jquery.fileupload-ui'
import 'bootstrap/dist/css/bootstrap.css'
import 'blueimp-file-upload/css/jquery.fileupload.css'
import 'blueimp-file-upload/css/jquery.fileupload-ui.css'

export default {
  name: 'fileupload',
  data() {
    return {
        msg: 'this is default message',
        option: {
            url: '/upload',
			autoUpload: false,
            uploadTemplateId: null,
            downloadTemplateId: null,
            uploadTemplate: function (o) {
				var rows = $();
				$.each(o.files, function (index, file) {
					var row = $('<tr class="template-upload fade">' +
						'<td><span class="preview"></span></td>' +
						'<td><p class="name"></p>' +
						'<div class="error"></div>' +
						'</td>' +
						'<td><p class="size"></p>' +
						'<div class="progress"></div>' +
						'</td>' +
						'<td>' +
						(!index && !o.options.autoUpload ?
							'<button type="button" class="btn btn-primary start" disabled>Start</button>' : '') +
						(!index ? '<button type="button" class="btn btn-warning cancel">Cancel</button>' : '') +
						'</td>' +
						'</tr>');
					row.find('.name').text(file.name);
					row.find('.size').text(o.formatFileSize(file.size));
					if (file.error) {
						row.find('.error').text(file.error);
					}
					rows = rows.add(row);
				});
				return rows;
			},
			downloadTemplate: function (o) {
				var rows = $();
				$.each(o.files, function (index, file) {
					var row = $('<tr class="template-download fade">' +
						'<td><span class="preview"></span></td>' +
						'<td><p class="name"></p>' +
						(file.error ? '<div class="error"></div>' : '') +
						'</td>' +
						'<td><span class="size"></span></td>' +
						'<td><button type="button" class="btn btn-danger delete">Delete</button></td>' +
						'</tr>');
					row.find('.size').text(o.formatFileSize(file.size));
					if (file.error) {
						row.find('.name').text(file.name);
						row.find('.error').text(file.error);
					} else {
						row.find('.name').append($('<a></a>').text(file.name));
						if (file.thumbnailUrl) {
							row.find('.preview').append(
								$('<a></a>').append(
									$('<img>').prop('src', file.thumbnailUrl)
								)
							);
						}
						row.find('a')
							.attr('data-gallery', '')
							.prop('href', file.url);
						row.find('button.delete')
							.attr('data-type', file.delete_type)
							.attr('data-url', file.delete_url);
					}
					rows = rows.add(row);
				});
				return rows;
			}
        }
    }
  },
  mounted() {
    $('#fileupload').fileupload(this.option)
  }
}
</script>
