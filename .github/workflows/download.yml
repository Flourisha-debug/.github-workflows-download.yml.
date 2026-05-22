name: Upload MP3 and Generate Release Links

on:
  workflow_dispatch:
    inputs:
      mp3_url:
        description: 'MP3 file URL to download'
        required: true
        type: string
      release_tag:
        description: 'Release tag (e.g., v1.0.0)'
        required: true
        type: string
      release_name:
        description: 'Release name'
        required: true
        type: string

jobs:
  upload-mp3:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Download MP3 file
        run: |
          mkdir -p mp3-files
          wget -O mp3-files/audio.mp3 "${{ github.event.inputs.mp3_url }}"
          ls -lh mp3-files/

      - name: Verify MP3 file
        run: |
          file mp3-files/audio.mp3
          du -h mp3-files/audio.mp3

      - name: Create Release
        id: create_release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ github.event.inputs.release_tag }}
          release_name: ${{ github.event.inputs.release_name }}
          body: |
            ## MP3 Audio File
            
            This release contains an MP3 audio file ready for download and use.
            
            **Download Links:**
            - Direct download: See "Assets" section below
            - Raw file URL: `https://github.com/${{ github.repository }}/releases/download/${{ github.event.inputs.release_tag }}/audio.mp3`
            
            **How to use in your web:**
            ```html
            <audio controls>
              <source src="https://github.com/${{ github.repository }}/releases/download/${{ github.event.inputs.release_tag }}/audio.mp3" type="audio/mpeg">
              Your browser does not support the audio element.
            </audio>
            ```
          draft: false
          prerelease: false

      - name: Upload MP3 to Release
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: ./mp3-files/audio.mp3
          asset_name: audio.mp3
          asset_content_type: audio/mpeg

      - name: Generate and Output Link Info
        run: |
          echo "✅ Release Created Successfully!"
          echo ""
          echo "📝 Release Information:"
          echo "  Tag: ${{ github.event.inputs.release_tag }}"
          echo "  Name: ${{ github.event.inputs.release_name }}"
          echo ""
          echo "🔗 Direct MP3 Link:"
          echo "  https://github.com/${{ github.repository }}/releases/download/${{ github.event.inputs.release_tag }}/audio.mp3"
          echo ""
          echo "🎵 HTML Audio Player:"
          echo '<audio controls><source src="https://github.com/${{ github.repository }}/releases/download/${{ github.event.inputs.release_tag }}/audio.mp3" type="audio/mpeg"></audio>'
          echo ""
          echo "📎 Release Page:"
          echo "  https://github.com/${{ github.repository }}/releases/tag/${{ github.event.inputs.release_tag }}"
