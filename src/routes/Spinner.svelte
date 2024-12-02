
  <div class="center">
    <div class="spinner"></div>
  </div>


  <style>
    .center{
      display: flex;
      justify-content: center;
      align-items: center;
      height: 60vh;
    }
    .spinner {
      border: 10px solid #0000001a;
      border-left-color: black;
      border-right: 1px solid #0000001a;
      border-bottom: 1px solid #0000001a;
      border-radius: 50%;
      width: 100px;
      height: 100px;
      animation: spin 1s linear infinite;
    }
  
    @keyframes spin {
      to {
        transform: rotate(360deg);
      }
    }
  </style>
  
