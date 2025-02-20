---
title: Lidarr Custom Scripts
description: 
published: true
date: 2022-05-02T03:37:28.132Z
tags: lidarr, needs-love, custom scripts
editor: markdown
dateCreated: 2021-11-24T19:22:09.331Z
---

If you're looking to trigger a custom script, you can find more details on this page. Scripts are added to Lidarr via the [Connect Settings](/lidarr/settings#connections).

# Overview

Lidarr can execute a custom script when an episode is newly imported or renamed. Depending on the action, different parameters are supplied. Parameters are passed to the script through environment variables.

## Lidarr Logs

Note that the following will only be logged for custom scripts:

- Script `stdout` output will be logged as `Debug`
- Script `stderr` output will be logged as `Info`
- The trigger of the script will be logged in `Trace`

# Environment Variables

Environment Variables vary based on the event type. The sections below indicate the variables available for each.

> [The sections below need cleanup, organizing, and details enhanced. View the source code here](https://github.com/Lidarr/Lidarr/blob/develop/src/NzbDrone.Core/Notifications/CustomScript/CustomScript.cs)
{.is-info}

## Grab

| Environment Variable               | Details                                                              |
| ---------------------------------- | -------------------------------------------------------------------- |
| `Lidarr_EventType`                 | Grab                                                                 |
| `Lidarr_Artist_Id`                 | `artist.Id`                                                          |
| `Lidarr_Artist_Name`               | `artist.Metadata.Value.Name`                                         |
| `Lidarr_Artist_MBId`               | `artist.Metadata.Value.ForeignArtistId`                              |
| `Lidarr_Artist_Type`               | `artist.Metadata.Value.Type`                                         |
| `Lidarr_Release_AlbumCount`        | `remoteAlbum.Albums.Count`                                           |
| `Lidarr_Release_AlbumReleaseDates` | Comma separated list of album release dates                          |
| `Lidarr_Release_AlbumTitles`       | Pipe separated list of album titles                                  |
| `Lidarr_Release_AlbumMBIds`        | Pipe separated list of album external service IDs (e.g. MusicBrainz) |
| `Lidarr_Release_Title`             | `remoteAlbum.Release.Title`                                          |
| `Lidarr_Release_Indexer`           | `remoteAlbum.Release.Indexer`                                        |
| `Lidarr_Release_Size`              | `remoteAlbum.Release.Size`                                           |
| `Lidarr_Release_Quality`           | `remoteAlbum.ParsedAlbumInfo.Quality.Quality.Name`                   |
| `Lidarr_Release_QualityVersion`    | `remoteAlbum.ParsedAlbumInfo.Quality.Revision.Version`               |
| `Lidarr_Release_ReleaseGroup`      | `releaseGroup`                                                       |
| `Lidarr_Download_Client`           | `message.DownloadClient`                                             |
| `Lidarr_Download_Id`               | `message.DownloadId`                                                 |

## On Import / On Upgrade

| Environment Variable       | Details                                  |
| -------------------------- | ---------------------------------------- |
| `Lidarr_EventType`         | AlbumDownload                            |
| `Lidarr_Artist_Id`         | `artist.Id`                              |
| `Lidarr_Artist_Name`       | `artist.Metadata.Value.Name`             |
| `Lidarr_Artist_Path`       | `artist.Path`                            |
| `Lidarr_Artist_MBId`       | `artist.Metadata.Value.ForeignArtistId`  |
| `Lidarr_Artist_Type`       | `artist.Metadata.Value.Type`             |
| `Lidarr_Album_Id`          | `album.Id`                               |
| `Lidarr_Album_Title`       | `album.Title`                            |
| `Lidarr_Album_MBId`        | `album.ForeignAlbumId`                   |
| `Lidarr_AlbumRelease_MBId` | `release.ForeignReleaseId`               |
| `Lidarr_Album_ReleaseDate` | `album.ReleaseDate`                      |
| `Lidarr_Download_Client`   | `message.DownloadClient`                 |
| `Lidarr_Download_Id`       | `message.DownloadId`                     |
| `Lidarr_AddedTrackPaths`   | Pipe separated list of added track paths |
| `Lidarr_DeletedPaths`      | Pipe separated list of deleted files     |

## Rename

| Environment Variable | Details                                 |
| -------------------- | --------------------------------------- |
| `Lidarr_EventType`   | Rename                                  |
| `Lidarr_Artist_Id`   | `artist.Id`                             |
| `Lidarr_Artist_Name` | `artist.Metadata.Value.Name`            |
| `Lidarr_Artist_Path` | `artist.Path`                           |
| `Lidarr_Artist_MBId` | `artist.Metadata.Value.ForeignArtistId` |
| `Lidarr_Artist_Type` | `artist.Metadata.Value.Type`            |

## Track Retag

| Environment Variable              | Details                                 |
| --------------------------------- | --------------------------------------- |
| `Lidarr_EventType`                | TrackRetag                              |
| `Lidarr_Artist_Id`                | `artist.Id`                             |
| `Lidarr_Artist_Name`              | `artist.Metadata.Value.Name`            |
| `Lidarr_Artist_Path`              | `artist.Path`                           |
| `Lidarr_Artist_MBId`              | `artist.Metadata.Value.ForeignArtistId` |
| `Lidarr_Artist_Type`              | `artist.Metadata.Value.Type`            |
| `Lidarr_Album_Id`                 | `album.Id`                              |
| `Lidarr_Album_Title`              | `album.Title`                           |
| `Lidarr_Album_MBId`               | `album.ForeignAlbumId`                  |
| `Lidarr_AlbumRelease_MBId`        | `release.ForeignReleaseId`              |
| `Lidarr_Album_ReleaseDate`        | `album.ReleaseDate`                     |
| `Lidarr_TrackFile_Id`             | `trackFile.Id`                          |
| `Lidarr_TrackFile_TrackCount`     | `trackFile.Tracks.Value.Count`          |
| `Lidarr_TrackFile_Path`           | `trackFile.Path`                        |
| `Lidarr_TrackFile_TrackNumbers`   | Comma separated list of track numbers   |
| `Lidarr_TrackFile_TrackTitles`    | Pipe separated list of track titles     |
| `Lidarr_TrackFile_Quality`        | `trackFile.Quality.Quality.Name`        |
| `Lidarr_TrackFile_QualityVersion` | `trackFile.Quality.Revision.Version`    |
| `Lidarr_TrackFile_ReleaseGroup`   | `trackFile.ReleaseGroup`                |
| `Lidarr_TrackFile_SceneName`      | `trackFile.SceneName`                   |
| `Lidarr_Tags_Diff`                | `message.Diff.ToJson()`                 |
| `Lidarr_Tags_Scrubbed`            | `message.Scrubbed`                      |

## Health Issue

| Environment Variable          | Details                                 |
| ----------------------------- | --------------------------------------- |
| `Lidarr_EventType`            | HealthIssue                             |
| `Lidarr_Health_Issue_Level`   | `nameof(healthCheck.Type)`              |
| `Lidarr_Health_Issue_Message` | `healthCheck.Message`                   |
| `Lidarr_Health_Issue_Type`    | `healthCheck.Source.Name`               |
| `Lidarr_Health_Issue_Wiki`    | Wiki URL for the health issue help page |

## On Test

When adding the script to Lidarr and clicking 'Test,' the script will be invoked with the following parameters. The script should be able to gracefully ignore any unsupported event type.

| Environment Variable | Details |
| -------------------- | ------- |
| `Lidarr_EventType`   | Test    |
