<script>
  export let name;
  export let author;
  export let image;
  export let length;
  export let link;
</script>

<div class="bookPerspectiveContainerStyles bookPerspectiveContainer">
  <div class="bookPerspectiveStyles bookPerspective">
    <div class="bookMetaTextStyles bookMetaText">
      <h3 class="bookMetaTextStylesTitle">{name}</h3>
      <p class="bookMetaTextStylesAuthor">{author}</p>
    </div>
    <div
      class="bookThreeDStyles bookThreeD"
      style="--book-height: {Math.floor(parseInt(length) / 18)}px"
    >
      <img src={image} alt={name} class="bookImage" />
    </div>
    <div class="bookExternalLink bookMetaText">
      <span class="bookExternalLinkSpan" on:click={window.open(link, "_blank")}>
        View on Amazon
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="1em"
          height="1em"
          fill="currentColor"
          viewBox="0 0 13 13"
          class="bookExternalLinkArrow"
        >
          <path
            d="M13 1.05a1 1 0 0 0-1-1L4 0a1 1 0 0 0 0 2h5.56l-8.27 8.29a1 1 0 0 0 .325 1.639 1 1 0 0 0 1.095-.219L11 3.42V9a1 1 0 0 0 2 0V1.05Z"
          ></path>
        </svg>
      </span>
    </div>
  </div>
</div>

<style>
  .bookPerspective {
    perspective: 500px;
  }

  .bookThreeD {
    transform-style: preserve-3d;
    transform-origin: center;
  }

  .bookThreeD:before {
    background-image: linear-gradient(
      90deg,
      #fff 40%,
      #f6f6f6 0,
      #f6f6f6 50%,
      #fff 0,
      #fff 90%,
      #f6f6f6 0,
      #f6f6f6
    );
    background-size: 3px 3px;
    transform: translateY(0) translateX(0) translateZ(0) rotateY(-90deg);
    transform-origin: right;
    width: var(--book-height, 30px);
  }

  .bookThreeD:after {
    background-image: linear-gradient(
      0deg,
      #f6f6f6 40%,
      #eaeaed 0,
      #eaeaed 50%,
      #f6f6f6 0,
      #f6f6f6 90%,
      #eaeaed 0,
      #eaeaed
    );
    background-size: 4px 4px;
    transform: translateY(0) translateX(0) translateZ(0) rotateX(-90deg);
    transform-origin: top;
    height: var(--book-height, 30px);
  }

  .bookPerspectiveContainer:hover .bookThreeD {
    transform: rotateX(80deg) rotate(45deg) scale(0.75);
  }

  .bookPerspectiveContainer:hover .bookMetaText {
    opacity: 100;
    transform: translateY(0);
  }

  .bookPerspectiveContainerStyles {
    display: block;
    cursor: pointer;
    border-radius: 16px;
    background-color: rgb(245, 245, 244);
    border: 1px solid rgb(229, 229, 229);
    padding: 48px;
    transition: background-color 0.5s;
    width: 100%;
  }

  .bookPerspectiveContainerStyles:hover {
    background-color: rgb(229, 229, 229);
  }

  :global(.dark) .bookPerspectiveContainerStyles {
    background-color: rgb(38, 38, 38);
    border: 1px solid rgb(64, 64, 64);
  }

  :global(.dark) .bookPerspectiveContainerStyles:hover {
    background-color: rgb(64, 64, 64);
  }

  .bookPerspectiveStyles {
    display: flex;
    height: 100%;
    justify-content: center;
    align-items: center;
    gap: 0;
    margin: 0;
  }

  .bookThreeDStyles {
    margin: 0;
    position: relative;
    transition: transform 0.5s ease-in-out;
    flex: auto 0;
    width: 100%;
    max-width: 120px;
  }

  .bookThreeDStyles:after {
    display: block;
    content: "";
    background-color: #f6f6f6;
    width: calc(100% + 0.5px);
    position: absolute;
    left: 0;
    border-radius: 8px 0 0 8px;
    border-color: #333;
    border-bottom: 3px solid;
    border-top: 3px solid;
    border-left: 4px solid;
    border-right: 0;
  }

  .bookThreeDStyles:before {
    display: block;
    content: "";
    background-color: #fff;
    height: calc(100% + 0.5px);
    position: absolute;
    right: 0;
    top: 0;
    border-color: #333;
    border-left: 3px solid;
    border-right: 3px solid;
    border-top: 3px solid #eaeaed;
    border-bottom: 0;
  }

  .bookMetaTextStyles {
    text-align: left;
    position: absolute;
    top: -24px;
    left: -24px;
    right: -24px;
    opacity: 0;
    transform: translateY(-16px);
    transition: all 0.5s ease-in-out;
    margin: 0;
  }

  .bookMetaTextStylesTitle {
    font-size: 18px !important;
    font-weight: bold;
    line-height: 1.2em;
    margin: 0 !important;
  }

  .bookMetaTextStylesAuthor {
    font-size: 16px;
    color: #666 !important;
    margin: 4px 0 0 !important;
  }

  :global(.dark) .bookMetaTextStylesTitle {
    color: #ffffff !important;
  }

  :global(.dark) .bookMetaTextStylesAuthor {
    color: rgb(178, 178, 178) !important;
  }

  .bookPerspectiveContainer:hover .bookImage {
    border-radius: 6px 0 0 6px;
  }

  .bookImage {
    border-radius: 3px 0 0 3px;
    transition: border-radius 0.3s ease-in-out;
  }

  .bookExternalLink {
    position: absolute;
    font-weight: bold;
    color: rgb(0, 0, 0) !important;
    font-size: 14px;
    bottom: -24px;
    left: -24px;
    right: -24px;
    opacity: 0;
    transform: translateY(16px);
    transition: all 0.5s ease-in-out;
    margin: 0;
    display: block;
  }

  :global(.dark) .bookExternalLink {
    color: #fff !important;
  }

  .bookExternalLinkSpan {
    display: inline-flex;
    align-items: center;
    margin-right: 4px;
    cursor: pointer;
    position: relative;
    text-decoration: underline dotted rgb(0, 0, 0);
    text-underline-offset: 6px;
    text-decoration-thickness: 2px;
  }

  :global(.dark) .bookExternalLinkSpan {
    text-decoration: underline dotted rgb(255, 255, 255);
  }

  .bookExternalLinkArrow {
    height: 8px;
    width: 8px;
    margin-left: 6px;
    fill: rgb(0, 0, 0);
  }

  :global(.dark) .bookExternalLinkArrow {
    fill: rgb(255, 255, 255);
  }
</style>
